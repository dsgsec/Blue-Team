# IOC

📝 IOC (Indicators of Compromise) & Sigma
-----------------------------------------

### Contexte

Lors d'une **réponse à incident**, les analystes identifient des **IOC (Indicators of Compromise)** : ce sont les traces laissées par un attaquant (domaine, IP, hash, URL, fichier...).

Ces IOC sont enregistrés dans une **feuille de suivi** afin de :

-   partager les informations entre analystes ;
-   faciliter les enquêtes ;
-   enrichir les mécanismes de détection.

> 💡 Un IOC découvert peut conduire à la découverte d'autres IOC associés.

* * * * *

### Threat Intelligence

Les IOC font partie de la **Threat Intelligence** : ils représentent des informations connues sur un attaquant ayant déjà compromis l'environnement.

➡️ Les réutiliser permet de **détecter rapidement une nouvelle intrusion du même adversaire**.

* * * * *

### Sigma

**Sigma** est un **langage open source de règles de détection**, indépendant du SIEM utilisé (Splunk, Sentinel, Elastic...).

Les IOC sont transformés en **règles Sigma** pour automatiser leur détection.

**Structure simplifiée :**

```
logsource    → source des logs
detection    → critères de détection
condition    → condition de déclenchement
```

**Exemple :**

-   Log proxy
-   Domaine = IOC connu
-   Téléchargement d'un `.exe`\
    → **Alerte générée**

* * * * *

⭐ À retenir
-----------

-   **IOC = trace d'une compromission.**
-   Les IOC sont stockés dans une feuille de suivi après un incident.
-   **1 IOC peut mener à plusieurs autres IOC.**
-   **Sigma transforme les IOC en règles de détection automatiques.**
-   **Plus il y a de couches de détection (IOC + Sigma + SIEM...), plus il est difficile pour un attaquant de passer inaperçu.**
