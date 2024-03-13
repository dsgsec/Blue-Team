Analyse judiciaire des disques
==============================

Comme nous l'avons souligné précédemment, il est crucial de respecter la séquence de volatilité des données. Il est impératif d'examiner chaque octet pour détecter les traces subtiles laissées par les cyber-adversaires. Après avoir abordé l'investigation de la mémoire, tournons maintenant notre attention vers le domaine de `disk forensics`(l'examen et l'analyse des images disque).

De nombreux outils d'analyse judiciaire des disques, à la fois commerciaux et open source, sont dotés de nombreuses fonctionnalités. Cependant, pour les équipes de réponse aux incidents, certaines fonctionnalités ressortent :

-   `File Structure Insight`: Être capable de naviguer et de voir la hiérarchie des fichiers du disque est crucial. Les outils médico-légaux de premier plan doivent afficher cette structure, permettant un accès rapide à des fichiers spécifiques, en particulier dans des emplacements connus sur un système suspect.
-   `Hex Viewer`: Pour les moments où vous avez besoin de vous rapprocher de vos données, la visualisation des fichiers en hexadécimal est essentielle. Cette fonctionnalité est particulièrement utile lorsqu'il s'agit de menaces telles que des logiciels malveillants sur mesure ou des exploits uniques.
-   `Web Artifacts Analysis`: Avec autant de données utilisateur liées aux activités Web, un outil médico-légal doit passer au crible et présenter efficacement ces données. Cela change la donne lorsque vous rassemblez des événements menant à l'arrivée d'un utilisateur sur un site Web malveillant.
-   `Email Carving`: Parfois, la piste mène à des menaces internes. Il s'agit peut-être d'un employé malhonnête ou simplement de quelqu'un qui a fait une erreur. Dans de tels cas, les e-mails détiennent souvent la clé. Un outil capable d'extraire et de présenter ces données rationalise le processus, facilitant ainsi la connexion entre les points.
-   `Image Viewer`: Parfois, les images stockées sur les systèmes peuvent raconter leur propre histoire. Qu'il s'agisse de vérifications de politiques ou d'analyses plus approfondies, disposer d'une visionneuse intégrée est une aubaine.
-   `Metadata Analysis`: Les détails tels que les horodatages de création de fichiers, les hachages et l'emplacement du disque peuvent être inestimables. Imaginez un scénario dans lequel vous essayez de faire correspondre l'heure de lancement d'une application avec une alerte de malware. De telles corrélations peuvent constituer le pivot de votre enquête.

Entrez dans [Autopsy](https://www.autopsy.com/) : une plate-forme médico-légale conviviale construite sur l'ensemble d'outils open source Sleuth Kit. Il reflète de nombreuses fonctionnalités que vous trouverez chez ses homologues commerciaux : évaluations de chronologies, recherches de mots clés, récupération d'artefacts Web et de courrier électronique et possibilité de passer au crible les résultats en fonction des hachages de fichiers malveillants connus.

Une fois que vous avez chargé une image médico-légale et traité les données, vous verrez les artefacts médico-légaux soigneusement organisés sur le panneau latéral. À partir de là, vous pouvez :

![](https://academy.hackthebox.com/storage/modules/237/image16.png)

-   Plongez dans `Data Sources`pour explorer les fichiers et les répertoires. ![](https://academy.hackthebox.com/storage/modules/237/image1_.png)

-   Examiner `Web Artifacts`. ![](https://academy.hackthebox.com/storage/modules/237/image80.png)

-   Vérifier `Attached Devices`. ![](https://academy.hackthebox.com/storage/modules/237/image8_.png)

-   Récupérer `Deleted Files`. ![](https://academy.hackthebox.com/storage/modules/237/image51.png)

-   Conduire `Keyword Searches`. ![](https://academy.hackthebox.com/storage/modules/237/image75.png)

    ![](https://academy.hackthebox.com/storage/modules/237/image23.png)

-   À utiliser `Keyword Lists`pour des recherches ciblées. ![](https://academy.hackthebox.com/storage/modules/237/image72.png)

    ![](https://academy.hackthebox.com/storage/modules/237/image52.png)

-   Entreprendre `Timeline Analysis`de cartographier les événements. ![](https://academy.hackthebox.com/storage/modules/237/image99.png)

* * * * *

Nous utiliserons largement l'autopsie dans la prochaine section « Scénario pratique de criminalistique numérique ».