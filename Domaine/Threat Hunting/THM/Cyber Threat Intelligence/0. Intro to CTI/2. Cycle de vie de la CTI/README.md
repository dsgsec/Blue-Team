Cycle de vie du CTI (6 phases) --- Scénario Alex / TryHatMe
=========================================================

Concepts préalables
-------------------

### Traffic Light Protocol (TLP)

Règle le partage des indicateurs. **Toujours faire voyager l'étiquette TLP avec l'indicateur** dans tout ticket/plateforme --- la violer casse la confiance avec les partenaires.

| Label | Limite de partage | Comportement L1 |
| --- | --- | --- |
| **CLEAR** | Aucune restriction | Poster sur le wiki interne |
| **GREEN** | Communauté de pairs, pas public | Slack restreint SOC partenaires |
| **AMBER** | Interne + clients need-to-know | Référencer sans copier dans les tickets |
| **RED** | Destinataires nommés uniquement | Note chiffrée, pas de ticket sans autorisation |

⚠️ **Règle de collision** : si deux sources donnent des labels différents pour le même indicateur → **le plus strict l'emporte** (ex: AMBER > CLEAR).

### Format

**STIX** = schéma structuré (lisible machine) pour décrire indicateurs, relations et contexte.

Le scénario : TryHatMe veut protéger sa base PostgreSQL de prod via CTI.
------------------------------------------------------------------------

* * * * *

Étape 1 --- Direction (définir la mission)
----------------------------------------

Traduire un mandat flou en besoin de renseignement concret :

-   **Actif** : serveur PostgreSQL prod
-   **Risque business** : amende RGPD + perte de confiance client
-   **Contrôles dispo** : NGFW (blocage IP/domaine), EDR (quarantaine par hash)
-   **2 questions directrices (Q1/Q2)** :
    -   Q1 : quelles IP/domaines externes ciblent PostgreSQL actuellement ?
    -   Q2 : quels malwares/hashes ciblent les credentials PostgreSQL cette semaine ?

→ Ces questions deviennent les **critères de succès** du cycle.

Étape 2 --- Collecte
------------------

4 sources combinées :

| Source | Justification | Exemple récolté |
| --- | --- | --- |
| Feed commercial NGFW | Haute fidélité | 37 IP exfil DB (24h) |
| AbuseIPDB (tag postgres-bruteforce) | Rapide, communautaire | 15 IP, 4 domaines |
| MISP interne | Historique incidents | 2 hash voleurs credentials |
| Rapport vendeur de la semaine | Stratégique → technique | 1 hash, 3 domaines |

→ Export en **STIX/CSV**, copie horodatée dans un bucket "raw-intel" (reproductibilité).

Étape 3 --- Traitement (Processing)
---------------------------------

**Normalisation** = format standard (IPv6 compressées, domaines minuscules, retrait des masques). **Corrélation** = dédupliquer / lier entre sources, repérer les contradictions.

Actions concrètes d'Alex :

-   Scripts Python planifiés : normalise → corrèle/déduplique vs plateforme existante → tag (source, date, **TLP**)
-   Génère 2 fichiers actionnables : `firewall_blocklist.csv` (NGFW) et `edr_hash_rules.yar` (EDR)
-   Cas de collision TLP géré ici (le plus strict gagne)

Étape 4 --- Analyse (jugement)
----------------------------

Éviter de bloquer aveuglément → risque de faux positifs. Croiser avec l'observation locale :

-   **Q1** : requête sur 30j → NGFW montre 5 tentatives échouées sur port 5432 → indicateur validé
-   **Q2** : OpenCTI relie le hash à **PgSteal** ; confirmé par sandbox Any.Run (credential dump) + driver ODBC concerné → priorité haute

**Matrice de confiance → action** :

| Confiance | Accord sources | Observation locale | Action |
| --- | --- | --- | --- |
| Haute | ≥2 sources concordantes | ≥1 tentative locale | **Blocage immédiat** |
| Moyenne | Source unique fiable | Aucun hit local | Alerte seulement |
| Basse | Source unique, pas de contexte | --- | Surveillance 14 jours |

Résultat : 7 IP + 1 hash → bloc immédiat ; le reste → surveillance.

Étape 5 --- Diffusion
-------------------

Adapter le format/niveau de détail **à chaque destinataire**, ni plus ni moins :

| Destinataire | Format | Pourquoi |
| --- | --- | --- |
| Équipe Firewall | CSV + ticket de changement | Appliquent les règles, documentent le risque/TLP |
| Équipe EDR | Règle YARA dans la console | Chargement en politique |
| Plateforme CTI | Objets indicateurs taggés | Historique, corrélations futures, respect TLP |
| Direction | Résumé ~200 mots | Démontre le ROI du processus |

Étape 6 --- Feedback (rétroaction)
--------------------------------

Mesurer après coup pour ajuster le prochain cycle :

| KPI | Avant | Après 1er cycle |
| --- | --- | --- |
| Temps de séjour médian (bruteforce PgSQL) | 48h | **0h** (blocage préventif) |
| Taux de faux positifs sur nouveaux blocs | --- | **0%** |

→ Résultats validés → direction approuve l'extension (ex: ajout d'IOC de tunneling) → nouvelle itération programmée.

* * * * *

🔑 Résumé ultra-condensé (les 6 phases)
---------------------------------------

1.  **Direction** --- définir la mission + questions clés
2.  **Collecte** --- rassembler depuis sources variées, exporter STIX/CSV
3.  **Traitement** --- normaliser, corréler, tagger TLP, produire fichiers actionnables
4.  **Analyse** --- valider par observation locale, noter la confiance, décider de l'action
5.  **Diffusion** --- adapter le livrable à chaque public
6.  **Feedback** --- mesurer les KPI, ajuster et relancer le cycle

**À retenir absolument** : le TLP le plus strict prévaut en cas de collision ; la matrice confiance/observation locale/action (étape 4) est le cœur de la prise de décision L1.
