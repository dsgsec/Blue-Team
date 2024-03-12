Présentation du processus de gestion des incidents
==================================================

Maintenant que nous connaissons la cyber kill chain et ses étapes, nous pouvons mieux prédire/anticiper les prochaines étapes d'une attaque et également suggérer des mesures appropriées pour les contrer.

Tout comme la cyber kill chain, la réponse à un incident comporte différentes étapes, définies comme la `incident handling process`. Le `incident handling process`définit une capacité permettant aux organisations de préparer, détecter et répondre aux événements malveillants. A noter que ce processus est adapté pour répondre aux événements de sécurité informatique, mais ses étapes ne correspondent pas aux étapes de la cyber kill chain de manière one-to-one.

Tel que défini par le NIST, le processus de gestion des incidents comprend les quatre (4) étapes distinctes suivantes : ![Processus de manipulation](https://academy.hackthebox.com/storage/modules/148/handling_process.png)

Les gestionnaires d'incidents passent la plupart de leur temps aux deux premières étapes, `preparation`et `detection & analysis`. C'est là que nous passons beaucoup de temps à nous améliorer et à rechercher le prochain événement malveillant. Lorsqu'un événement malveillant est détecté, nous passons alors à l'étape suivante et répondons à l'événement `(but there should always be resources operating on the first two stages, so that there is no disruption of preparation and detection capabilities)`. Comme vous pouvez le voir sur l'image, le processus n'est pas linéaire mais cyclique. Le principal point à comprendre à ce stade est qu'à mesure que de nouvelles preuves sont découvertes, les prochaines étapes peuvent également changer. Il est essentiel de s'assurer de ne pas sauter d'étapes dans le processus et de terminer une étape avant de passer à la suivante. Par exemple, si vous découvrez dix machines infectées, vous ne devriez certainement pas en contenir seulement cinq et commencer leur éradication pendant que les cinq autres restent infectées. Une telle approche peut s'avérer inefficace car, au minimum, vous informez un attaquant que vous l'avez découvert et que vous le traquez, ce qui, comme vous pouvez l'imaginer, peut avoir des conséquences imprévisibles.

Ainsi, la gestion des incidents comporte deux activités principales, qui sont `investigating`et `recovering`. L'enquête vise à :

-   Découvrez la victime initiale du « patient zéro » et créez une chronologie des incidents (en cours si toujours active).
-   Déterminer les outils et les logiciels malveillants utilisés par l'adversaire
-   Documentez les systèmes compromis et ce que l'adversaire a fait

À la suite de l'enquête, l'activité de rétablissement consiste à créer et à mettre en œuvre un plan de rétablissement. Une fois le plan mis en œuvre, l'entreprise devrait reprendre ses activités normales, si l'incident a provoqué des perturbations.

Lorsqu'un incident est entièrement traité, un rapport est émis qui détaille la cause et le coût de l'incident. De plus, des activités de « leçons apprises » sont réalisées, entre autres, pour comprendre ce que l'organisation doit faire pour éviter que des incidents de type similaire ne se reproduisent.

Laissez-nous maintenant vous guider à travers toutes les étapes du `incident handling process`.