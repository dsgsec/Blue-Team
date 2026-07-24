Tripwires --- Détection basée sur les leurres
===========================================

Concept clé
-----------

-   **Tripwire** = mécanisme complémentaire aux détections classiques, utile pour couvrir les "inconnues inconnues" et étudier un adversaire.
-   Repose sur l'exploitation de **données ultra-sensibles / accès restreints** : toute interaction avec ces éléments est anormale par nature → alerte immédiate.
-   Deux types principaux :
    -   **Honeypots** : ressources qui n'ont *aucune* utilité légitime → toute activité dessus = signal fort.
    -   **Fichiers/dossiers cachés** : invisibles pour un utilisateur normal → très efficaces contre les crawlers automatisés (vers, bots).

Mise en place sous Windows (audit natif)
----------------------------------------

### 1\. Activer l'audit d'accès aux objets (niveau système)

-   `secpol.msc` (Politique de sécurité locale)
-   Paramètres de sécurité → Politiques locales → **Politique d'audit**
-   Ouvrir **"Auditer l'accès aux objets"** → cocher **Réussite** ET **Échec** → Appliquer/OK
-   ⚠️ Sans cette étape globale, rien ne sera loggé même si l'audit est configuré sur un fichier précis.

### 2\. Créer l'appât

-   Créer un fichier "piège" (ex: `Document secret.txt`) sur le bureau ou, mieux, **dans un dossier dédié** si plusieurs fichiers à surveiller (facilite la gestion).

### 3\. Configurer l'audit sur l'objet précis

-   Clic droit sur le fichier → Propriétés → Sécurité → **Avancé** → onglet **Audit**
-   Ajouter → Sélectionner un principal → taper **"Tout le monde"** (Everyone)
-   Choisir les types d'accès à auditer (lecture, modif, suppression...) --- possibilité d'aller plus fin via "Afficher les autorisations avancées"
-   OK → Appliquer → OK (à chaque niveau de fenêtre)

Résultat
--------

-   Tout accès (réussi ou non) au fichier génère un événement dans le **journal des événements de sécurité**
-   **Event ID clé : 4663** (accès à un objet audité)
-   Ces événements sont filtrables/corrélables pour déclencher des alertes SOC exploitables immédiatement.

Points à retenir pour la pratique
---------------------------------

-   L'audit doit être activé à **deux niveaux** : politique globale (secpol) + objet spécifique (ACL d'audit du fichier/dossier).
-   Nommer les leurres de façon crédible ("carte au trésor" → ex. "Document secret", "Salaires_2024.xlsx", etc.)
-   Grouper les fichiers sensibles dans un dossier unique si plusieurs leurres → audit plus simple à gérer.
-   Event ID 4663 = le réflexe à retenir pour bâtir des règles de détection/SIEM autour des tripwires Windows.
