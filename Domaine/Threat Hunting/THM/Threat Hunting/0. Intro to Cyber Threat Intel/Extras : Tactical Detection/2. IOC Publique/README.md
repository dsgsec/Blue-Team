📝 IOC publics & Règles Sigma
-----------------------------

### Contexte

Lorsqu'une **nouvelle vulnérabilité (0-day)** est découverte, la communauté cybersécurité publie souvent :

-   des **IOC publics** ;
-   des **règles Sigma** prêtes à l'emploi.

➡️ Même sans avoir subi l'attaque, on peut utiliser ces ressources pour protéger son environnement.

* * * * *

### Rôle des règles Sigma

Les règles Sigma permettent de **détecter l'exploitation d'une vulnérabilité** à partir des IOC publiés.

Exemples :

-   **Follina (MSDT)** → détecte une exécution suspecte de `msdt.exe`.
-   **Log4Shell (Log4j)** → détecte des processus suspects lancés par `java.exe`.

* * * * *

### Uncoder

**Uncoder** est un outil qui **convertit une règle Sigma** vers le format d'un SIEM (ex. : ElastAlert, Elastic...).

Fonctionnement :

1.  Copier une règle Sigma.
2.  La coller dans Uncoder.
3.  Choisir le format de sortie (ElastAlert, Splunk, etc.).
4.  Générer la règle adaptée.

> ⚠️ La règle générée est une **base** : elle doit être **testée et adaptée** avant une mise en production.

* * * * *

⭐ À retenir
-----------

-   Les **IOC publics** sont publiés par la communauté après la découverte de nouvelles menaces.
-   Les **règles Sigma** permettent de transformer ces IOC en **alertes de détection**.
-   **Uncoder** convertit une règle Sigma vers le format d'un SIEM.
-   Les règles générées doivent toujours être **validées et ajustées** avant d'être déployées.
