Étape de confinement, d'éradication et de récupération
======================================================

Lorsque l'enquête est terminée et que nous avons compris le type d'incident et l'impact sur l'entreprise (sur la base de toutes les pistes recueillies et des informations rassemblées dans la chronologie), il est temps d'entrer dans la phase de confinement pour éviter que l'incident n'en provoque davantage. dommage.

* * * * *

Endiguement
-----------

À ce stade, nous prenons des mesures pour empêcher la propagation de l'incident. Nous divisons les actions en `short-term containment`et `long-term containment`. Il est important que les mesures de confinement soient coordonnées et exécutées simultanément dans tous les systèmes. Sinon, nous risquons d'informer les attaquants que nous les poursuivons, auquel cas ils pourraient modifier leurs techniques et leurs outils afin de persister dans l'environnement.

En confinement à court terme, les actions entreprises laissent une empreinte minimale sur les systèmes sur lesquels elles surviennent. Certaines de ces actions peuvent inclure le placement d'un système dans un VLAN séparé/isolé, le retrait du câble réseau du ou des systèmes ou la modification du nom DNS C2 de l'attaquant en un système sous notre contrôle ou en un système inexistant. Les actions ici contiennent les dégâts et donnent le temps de développer une stratégie de réparation plus concrète. De plus, puisque nous maintenons les systèmes inchangés (autant que possible), nous avons la possibilité de prendre des images médico-légales et de conserver des preuves si cela n'a pas déjà été fait au cours de l'enquête (c'est également ce qu'on appelle la sous- `backup`étape de l'étape de confinement). Si une mesure de confinement à court terme nécessite l'arrêt d'un système, nous devons nous assurer que cela est communiqué à l'entreprise et que les autorisations appropriées sont accordées.

Dans les actions de confinement à long terme, nous nous concentrons sur les actions et les changements persistants. Celles-ci peuvent inclure la modification des mots de passe des utilisateurs, l'application de règles de pare-feu, l'insertion d'un système de détection d'intrusion sur l'hôte, l'application d'un correctif système et l'arrêt des systèmes. Tout en effectuant ces activités, nous devons tenir l'entreprise et les parties prenantes concernées informées. Gardez à l'esprit que ce n'est pas parce qu'un système est désormais corrigé que l'incident est terminé. Les activités d'éradication, de rétablissement et post-incident sont toujours en cours.

* * * * *

Éradication
-----------

Une fois l'incident maîtrisé, l'éradication est nécessaire pour éliminer à la fois la cause profonde de l'incident et ce qui en reste afin de garantir que l'adversaire soit hors des systèmes et du réseau. Certaines des activités de cette étape incluent la suppression des logiciels malveillants détectés des systèmes, la reconstruction de certains systèmes et la restauration d'autres à partir d'une sauvegarde. Au cours de la phase d'éradication, nous pouvons étendre les activités de confinement précédemment réalisées en appliquant des correctifs supplémentaires, qui n'étaient pas immédiatement nécessaires. Des activités supplémentaires de renforcement du système sont souvent effectuées pendant la phase d'éradication (non seulement sur le système concerné mais dans certains cas sur l'ensemble du réseau).

* * * * *

Récupération
------------

Lors de la phase de récupération, nous remettons les systèmes en fonctionnement normal. Bien entendu, l'entreprise doit vérifier qu'un système fonctionne effectivement comme prévu et qu'il contient toutes les données nécessaires. Lorsque tout est vérifié, ces systèmes sont introduits dans l'environnement de production. Tous les systèmes restaurés seront soumis à une journalisation et à une surveillance intensives après un incident, car les systèmes compromis ont tendance à être à nouveau des cibles si l'adversaire retrouve l'accès à l'environnement dans un court laps de temps. Les événements suspects typiques à surveiller sont :

-   Connexions inhabituelles (par exemple, comptes d'utilisateurs ou de services qui ne s'y sont jamais connectés auparavant)
-   Processus inhabituels
-   Modifications apportées au registre dans des emplacements généralement modifiés par des logiciels malveillants

Dans le cas de certains incidents majeurs, la phase de rétablissement peut prendre des mois, car elle est souvent abordée par étapes. Au cours des premières phases, l'accent est mis sur l'augmentation de la sécurité globale afin de prévenir de futurs incidents grâce à des résultats rapides et à l'élimination des fruits les plus faciles à trouver. Les phases ultérieures se concentrent sur des changements permanents à long terme pour maintenir l'organisation aussi sécurisée que possible.