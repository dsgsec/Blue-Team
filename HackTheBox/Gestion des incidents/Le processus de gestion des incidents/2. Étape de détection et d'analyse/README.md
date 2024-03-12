Étape de détection et d'analyse (partie 1)
==========================================

À ce stade, nous avons créé des processus et des procédures, et nous avons des lignes directrices sur la manière d'agir en cas d'incidents de sécurité.

Cette `detection & analysis`phase implique tous les aspects de la détection d'un incident, tels que l'utilisation de capteurs, de journaux et de personnel qualifié. Cela inclut également le partage d'informations et de connaissances, ainsi que l'utilisation de renseignements sur les menaces basés sur le contexte. La segmentation de l'architecture et une compréhension claire et une visibilité au sein du réseau sont également des facteurs importants.

Les menaces sont introduites dans l'organisation via une quantité infinie de vecteurs d'attaque, et leur détection peut provenir de sources telles que :

-   Un employé qui constate un comportement anormal
-   Une alerte depuis un de nos outils (EDR, IDS, Firewall, SIEM, etc.)
-   Activités de chasse aux menaces
-   Une notification tierce nous informant qu'ils ont découvert des signes indiquant que notre organisation était compromise

* * * * *

Il est fortement recommandé de créer des niveaux de détection en catégorisant logiquement notre réseau comme suit.

-   Détection au périmètre du réseau (à l'aide de pare-feu, de systèmes de détection/prévention des intrusions sur le réseau, de zones démilitarisées, etc.)
-   Détection au niveau du réseau interne (à l'aide de pare-feu locaux, de systèmes de détection/prévention des intrusions sur l'hôte, etc.)
-   Détection au niveau du point final (à l'aide de systèmes antivirus, de systèmes de détection et de réponse des points finaux, etc.)
-   Détection au niveau de l'application (à l'aide des journaux d'application, des journaux de service, etc.)

* * * * *

Enquête initiale
----------------

Lorsqu'un incident de sécurité est détecté, vous devez mener une enquête initiale et établir le contexte avant de constituer l'équipe et d'appeler une réponse à l'incident à l'échelle de l'organisation. Pensez à la façon dont les informations sont présentées dans le cas où un compte administratif se connecte à une adresse IP à HH:MM:SS. Sans savoir quel système se trouve sur cette adresse IP et à quel fuseau horaire l'heure se réfère, nous pouvons facilement tirer une conclusion erronée sur l'objet de cet événement. En résumé, nous devrions chercher à collecter autant d'informations que possible à ce stade sur les éléments suivants :

-   Date/heure à laquelle l'incident a été signalé. De plus, qui a détecté l'incident et/ou qui l'a signalé ?
-   Comment l'incident a-t-il été détecté ?
-   Quel a été l'incident ? Hameçonnage? Indisponibilité du système ? etc.
-   Dressez une liste des systèmes concernés (le cas échéant)
-   Documentez qui a accédé aux systèmes concernés et quelles mesures ont été prises. Notez s'il s'agit d'un incident en cours ou si l'activité suspecte a été arrêtée
-   Emplacement physique, systèmes d'exploitation, adresses IP et noms d'hôte, propriétaire du système, objectif du système, état actuel du système
-   (Si un logiciel malveillant est impliqué) Liste des adresses IP, heure et date de détection, type de logiciel malveillant, systèmes impactés, exportation de fichiers malveillants contenant des informations médico-légales (telles que des hachages, des copies des fichiers, etc.)

Avec ces informations à portée de main, nous pouvons prendre des décisions basées sur les connaissances que nous avons acquises. Qu'est-ce que cela signifie? Nous prendrions probablement des mesures différentes si nous savions que l'ordinateur portable du PDG était compromis plutôt que celui d'un stagiaire.

Avec les informations initialement recueillies, nous pouvons commencer à établir une chronologie des incidents. Cette chronologie nous permettra de rester organisés tout au long de l'événement et de fournir une image globale de ce qui s'est passé. Les événements de la chronologie sont triés dans le temps en fonction du moment où ils se sont produits. Notez qu'au cours du processus d'enquête ultérieur, nous ne découvrirons pas nécessairement des preuves dans cet ordre chronologique. Cependant, lorsque nous trions les preuves en fonction du moment où ils se sont produits, nous obtiendrons le contexte des différents événements qui ont eu lieu. La chronologie peut également permettre de déterminer si les preuves nouvellement découvertes font partie de l'incident actuel. Par exemple, imaginez que ce que nous pensions être la charge utile initiale d'une attaque soit découvert plus tard sur un autre appareil il y a deux semaines. Nous rencontrerons des situations où les données que nous examinons sont extrêmement pertinentes et des situations où les données ne sont pas liées et où nous cherchons au mauvais endroit. Dans l'ensemble, la chronologie doit contenir les informations décrites dans les colonnes suivantes :

| `Date` | `Time of the event` | `hostname` | `event description` | `data source` |
| --- | --- | --- | --- | --- |

Prenons un événement et remplissons le tableau d'exemple ci-dessus. Cela ressemblera à ceci :

| `Date` | `Time of the event` | `hostname` | `event description` | `data source` |
| --- | --- | --- | --- | --- |
| 09/09/2021 | 13h31 (heure de Paris) | SQLServeur01 | L'outil de piratage 'Mimikatz' a été détecté | Logiciel antivirus |

Comme vous pouvez le déduire, la chronologie se concentre principalement sur le comportement de l'attaquant, de sorte que les activités enregistrées décrivent le moment où l'attaque s'est produite, le moment où une connexion réseau a été établie pour accéder à un système, le moment où les fichiers ont été téléchargés, etc. Il est important de vous assurer que vous capturez d'où l'activité a été détectée/découverte et les systèmes qui y sont associés.

* * * * *

Questions sur la gravité et l'étendue des incidents
---------------------------------------------------

Lors du traitement d'un incident de sécurité, nous devons également essayer de répondre aux questions suivantes pour avoir une idée de la gravité et de l'étendue de l'incident :

-   Quel est l'impact de l'exploitation ?
-   Quelles sont les conditions d'exploitation ?
-   Des systèmes critiques pour l'entreprise peuvent-ils être affectés par l'incident ?
-   Existe-t-il des suggestions de mesures correctives ?
-   Combien de systèmes ont été impactés ?
-   L'exploit est-il utilisé dans la nature ?
-   L'exploit a-t-il des capacités semblables à celles d'un ver ?

Les deux derniers peuvent éventuellement indiquer le niveau de sophistication d'un adversaire.

Comme vous pouvez l'imaginer, les incidents à fort impact seront traités rapidement et les incidents impliquant un nombre élevé de systèmes concernés devront être remontés.

* * * * *

Confidentialité et communication des incidents
----------------------------------------------

Les incidents sont des sujets très confidentiels et, à ce titre, toutes les informations recueillies doivent être conservées en fonction du besoin d'en connaître, à moins que les lois applicables ou une décision de la direction nous instruisent autrement. Il y a plusieurs raisons à cela. L'adversaire peut être, par exemple, un employé de l'entreprise, ou en cas de violation, la communication aux parties internes et externes doit être traitée par la personne désignée en accord avec le service juridique.

Lorsqu'une enquête est lancée, nous fixerons certaines attentes et objectifs. Ceux-ci incluent souvent le type d'incident survenu, les sources de preuves dont nous disposons et une estimation approximative du temps dont l'équipe a besoin pour l'enquête. De plus, en fonction de l'incident, nous définirons nos attentes quant à savoir si nous serons en mesure de découvrir ou non l'adversaire. Bien entendu, une grande partie de ce qui précède peut changer à mesure que l'enquête évolue et que de nouvelles pistes sont découvertes. Il est important de tenir tout le monde impliqué et la direction informée de toute avancée et attente.


---

Étape de détection et d'analyse (partie 2)
==========================================

Lorsqu'une enquête est ouverte, notre objectif est de comprendre ce qui s'est produit et comment. Pour analyser correctement et efficacement les données liées aux incidents, les membres de l'équipe de gestion des incidents ont besoin de connaissances techniques approfondies et d'une expérience dans le domaine. On peut se demander : « Pourquoi nous soucions-nous de la façon dont un incident s'est produit ? Pourquoi ne pas simplement reconstruire les systèmes touchés et oublier qu'il s'est produit ?

Si nous ne savons pas comment un incident s'est produit ni ce qui a été impacté, les mesures correctives que nous prenons ne garantiront pas que l'attaquant ne puisse pas répéter ses actions pour retrouver l'accès. Si, en revanche, nous savons exactement comment l'adversaire est entré, quels outils il a utilisé et quels systèmes ont été touchés, nous pouvons alors planifier nos mesures correctives pour garantir que cette voie d'attaque ne puisse pas être reproduite.

* * * * *

L'enquête
---------

L'enquête démarre sur la base des informations initialement recueillies (et limitées) qui contiennent ce que nous savons jusqu'à présent sur l'incident. Avec ces données initiales, nous commencerons un processus cyclique en 3 étapes qui se répétera encore et encore au fur et à mesure de l'évolution de l'enquête. Ce processus comprend :

-   Création et utilisation d'indicateurs de compromission (IOC)
-   Identification des nouveaux leads et des systèmes impactés
-   Collecte et analyse des données des nouveaux leads et des systèmes impactés

![Enquête](https://academy.hackthebox.com/storage/modules/148/investigation_new.png)

Développons maintenant davantage le processus décrit ci-dessus.

* * * * *

### Données d'enquête initiales

Afin de parvenir à une conclusion, une enquête doit s'appuyer sur des pistes valables qui ont été découvertes non seulement au cours de cette phase initiale mais tout au long du processus d'enquête. L'équipe de gestion des incidents doit constamment proposer de nouvelles pistes et ne pas s'attaquer uniquement à une découverte spécifique, telle qu'un outil malveillant connu. Limiter une enquête à une activité spécifique aboutit souvent à des résultats limités, à des conclusions prématurées et à une compréhension incomplète de l'impact global.

* * * * *

### Création et utilisation des IOC

Un indicateur de compromission est le signe qu'un incident s'est produit. Les IOC sont documentés de manière structurée, ce qui représente les artefacts du compromis. Des exemples d'IOC peuvent être des adresses IP, des valeurs de hachage de fichiers et des noms de fichiers. En fait, parce que les IOC sont si importants pour une enquête, des langages spéciaux tels qu'OpenIOC ont été développés pour les documenter et les partager de manière standard. Yara est une autre norme largement utilisée pour les IOC. Il existe un certain nombre d'outils gratuits qui peuvent être utilisés, tels que l'éditeur IOC de Mandiant, pour créer ou modifier des IOC. Grâce à ces langages, nous pouvons décrire et utiliser les artefacts que nous découvrons lors d'une enquête sur un incident. Nous pouvons même obtenir des IOC auprès de tiers si l'adversaire ou l'attaque est connu.

Pour tirer parti des IOC, nous devrons déployer un outil d'obtention/de recherche d'IOC (natif ou tiers et éventuellement à grande échelle). Une approche courante consiste à utiliser WMI ou PowerShell pour les opérations liées à IOC dans les environnements Windows. Un mot d'avertissement! Au cours d'une enquête, nous devons être très prudents pour éviter que les informations d'identification de nos utilisateurs hautement privilégiés ne soient mises en cache lors de la connexion à des systèmes (potentiellement) compromis (ou à n'importe quel système, en fait). Plus précisément, nous devons nous assurer que seuls les protocoles et outils de connexion qui ne mettent pas en cache les informations d'identification lors d'une connexion réussie sont utilisés (tels que WinRM). Les connexions Windows avec le type de connexion 3 (connexion réseau) ne mettent généralement pas en cache les informations d'identification sur les systèmes distants. Le meilleur exemple de « connaître vos outils » qui me vient à l'esprit est « PsExec ». Lorsque « PsExec » est utilisé avec des informations d'identification explicites, ces informations d'identification sont mises en cache sur la machine distante. Lorsque « PsExec » est utilisé sans informations d'identification via la session de l'utilisateur actuellement connecté, les informations d'identification ne sont pas mises en cache sur la machine distante. Il s'agit d'un excellent exemple montrant comment le même outil laisse des traces différentes, alors soyez-en conscient.

* * * * *

### Identification des nouveaux prospects et des systèmes impactés

Après avoir recherché des IOC, vous vous attendez à recevoir des résultats révélant d'autres systèmes présentant les mêmes signes de compromission. Ces résultats peuvent ne pas être directement associés à l'incident sur lequel nous enquêtons. Notre CIO pourrait par exemple être trop générique. Nous devons identifier et éliminer les faux positifs. Nous pouvons également nous retrouver dans une position où nous rencontrons un grand nombre de hits. Dans ce cas, nous devons prioriser ceux sur lesquels nous nous concentrerons, idéalement ceux qui peuvent nous fournir de nouvelles pistes après une éventuelle analyse médico-légale.

* * * * *

### Collecte et analyse de données à partir des nouveaux prospects et des systèmes impactés

Une fois que nous aurons identifié les systèmes qui incluent nos IOC, nous souhaiterons collecter et préserver l'état de ces systèmes pour une analyse plus approfondie afin de découvrir de nouvelles pistes et/ou de répondre aux questions d'enquête sur l'incident. Selon le système, il existe plusieurs approches quant à la manière et aux données à collecter. Parfois, nous souhaitons effectuer une « réponse en direct » sur un système pendant qu'il est en cours d'exécution, tandis que dans d'autres cas, nous souhaitons peut-être arrêter un système, puis effectuer une analyse sur celui-ci. La réponse en direct est l'approche la plus courante, dans laquelle nous collectons un ensemble prédéfini de données généralement riches en artefacts pouvant expliquer ce qui est arrivé à un système. Arrêter un système n'est pas une décision facile lorsqu'il s'agit de préserver des informations précieuses car, dans de nombreux cas, la plupart des artefacts ne vivront que dans la mémoire RAM de la machine, qui sera perdue si la machine est éteinte. Quelle que soit l'approche de collecte que nous choisissons, il est essentiel de garantir une interaction minimale avec le système afin d'éviter d'altérer les preuves ou les artefacts.

Une fois les données collectées, il est temps de les analyser. Il s'agit souvent du processus le plus long lors d'un incident. L'analyse des logiciels malveillants et l'investigation des disques sont les types d'examen les plus courants. Toutes les pistes nouvellement découvertes et validées sont ajoutées à la chronologie, qui est constamment mise à jour. Notez également que l'investigation de la mémoire est une fonctionnalité de plus en plus populaire et extrêmement pertinente lorsqu'il s'agit d'attaques avancées.

Gardez à l'esprit que pendant le processus de collecte de données, vous devez suivre la chaîne de traçabilité afin de garantir que les données examinées sont admissibles devant le tribunal si une action en justice doit être engagée contre un adversaire.