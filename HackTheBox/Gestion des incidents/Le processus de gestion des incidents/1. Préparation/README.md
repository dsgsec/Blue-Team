Étape de préparation (partie 1)
===============================

Dans la `preparation`scène, nous avons deux objectifs distincts. Le premier est la mise en place d'une capacité de gestion des incidents au sein de l'organisation. La seconde est la capacité à se protéger et à prévenir les incidents de sécurité informatique en mettant en œuvre des mesures de protection appropriées. Ces mesures incluent le renforcement des points de terminaison et des serveurs, la hiérarchisation d'Active Directory, l'authentification multifacteur, la gestion des accès privilégiés, etc. Bien que la protection contre les incidents ne relève pas de la responsabilité de l'équipe de gestion des incidents, cette activité est fondamentale au succès global de cette équipe.

* * * * *

Conditions préalables à la préparation
--------------------------------------

Lors de la préparation, nous devons nous assurer d'avoir :

-   Membres qualifiés de l'équipe de gestion des incidents (les membres de l'équipe de gestion des incidents peuvent être externalisés, mais une capacité et une compréhension de base de la gestion des incidents sont néanmoins nécessaires en interne)
-   Main-d'œuvre formée (autant que possible, par le biais d'activités de sensibilisation à la sécurité ou d'autres moyens de formation)
-   Politiques et documentation claires
-   Outils (logiciels et matériels)

* * * * *

Politiques et documentation claires
-----------------------------------

Certaines politiques et documents écrits doivent contenir une version à jour des informations suivantes :

-   Coordonnées et rôles des membres de l'équipe de gestion des incidents
-   Coordonnées du service juridique et de conformité, de l'équipe de direction, du support informatique, du service des communications et des relations avec les médias, des forces de l'ordre, des fournisseurs de services Internet, de la gestion des installations et de l'équipe externe de réponse aux incidents.
-   Politique, plan et procédures de réponse aux incidents
-   Politique et procédures de partage d'informations sur les incidents
-   Bases de référence des systèmes et des réseaux, à partir d'une image dorée et d'un environnement propre
-   Diagrammes de réseau
-   Base de données de gestion des actifs à l'échelle de l'organisation
-   Comptes d'utilisateurs dotés de privilèges excessifs qui peuvent être utilisés à la demande par l'équipe lorsque cela est nécessaire (également sur des systèmes critiques pour l'entreprise, qui sont gérés avec les compétences nécessaires pour administrer ce système spécifique). Ces comptes d'utilisateurs sont normalement activés lorsqu'un incident est confirmé lors de l'enquête initiale, puis désactivés une fois celle-ci terminée. Une réinitialisation obligatoire du mot de passe est également effectuée lors de la désactivation des utilisateurs.
-   Possibilité d'acquérir du matériel, des logiciels ou une ressource externe sans processus d'approvisionnement complet (achat urgent jusqu'à un certain montant). La dernière chose dont vous avez besoin lors d'un incident est d'attendre des semaines l'approbation d'un outil à 500 $.
-   Aide-mémoire pour la médecine légale et les enquêtes

Certains cas non graves peuvent être traités relativement rapidement et sans trop de frictions au sein ou à l'extérieur de l'organisation. D'autres cas peuvent nécessiter une notification aux forces de l'ordre et une communication externe aux clients et aux fournisseurs tiers, notamment en cas de problèmes juridiques découlant de l'incident. Par exemple, une violation de données impliquant des données client doit être signalée aux forces de l'ordre dans un certain délai, conformément au RGPD. Il peut y avoir de nombreuses exigences de conformité en fonction du lieu et/ou des succursales où l'incident s'est produit. La meilleure façon de les comprendre est donc d'en discuter avec vos équipes juridiques et de conformité pour chaque incident (ou de manière proactive).

S'il est essentiel de disposer d'une documentation, il est également important de documenter l'incident lors de votre enquête. Par conséquent, au cours de cette étape, vous devrez également établir une capacité de reporting efficace. Les incidents peuvent être extrêmement stressants, et il devient facile d'oublier cette partie à mesure que l'incident se déroule, surtout lorsque vous êtes concentré et que vous allez extrêmement vite afin de le résoudre le plus rapidement possible. Essayez de rester calme, prenez des notes et assurez-vous que ces notes contiennent des horodatages, l'activité effectuée, le résultat et qui l'a fait. Dans l'ensemble, vous devez chercher des réponses sur qui, quoi, quand, où, pourquoi et comment.

* * * * *

Outils (logiciels et matériels)
-------------------------------

À l'avenir, nous devons également nous assurer que nous disposons des bons outils pour effectuer le travail. Ceux-ci incluent, sans toutefois s'y limiter :

-   Un ordinateur portable supplémentaire ou un poste de travail médico-légal pour chaque membre de l'équipe de gestion des incidents afin de conserver les images disque et les fichiers journaux, d'effectuer des analyses de données et d'enquêter sans aucune restriction (nous savons que les logiciels malveillants seront testés ici, les outils tels que l'antivirus doivent donc être désactivés). Ces appareils doivent être manipulés de manière appropriée et non d'une manière qui présente des risques pour l'organisation.
-   Outils d'acquisition et d'analyse d'images médico-légales numériques
-   Outils de capture et d'analyse de la mémoire
-   Capture et analyse des réponses en direct
-   Outils d'analyse des journaux
-   Outils de capture et d'analyse du réseau
-   Câbles et commutateurs réseau
-   Bloqueurs d'écriture
-   Disques durs pour l'imagerie médico-légale
-   Câbles d'alimentation
-   Tournevis, pincettes et autres outils pertinents pour réparer ou démonter les périphériques matériels si nécessaire
-   Créateur d'indicateurs de compromission (IOC) et possibilité de rechercher des IOC dans toute l'organisation
-   Formulaires de chaîne de traçabilité
-   Logiciel de cryptage
-   Système de suivi des billets
-   Installation sécurisée pour le stockage et l'enquête
-   Système de gestion des incidents indépendant de l'infrastructure de votre organisation

La plupart des outils mentionnés ci-dessus feront partie de ce que l'on appelle un `jump bag`- toujours prêt avec les outils nécessaires à récupérer et à partir immédiatement. Sans ce sac préparé, rassembler tous les outils nécessaires à la volée peut prendre des jours, voire des semaines, avant que vous soyez prêt à répondre.

Enfin, nous souhaitons souligner l'importance d'avoir votre système de documentation complètement indépendant de l'infrastructure de votre organisation et correctement sécurisé. Supposons dès le début que l'ensemble de votre domaine est compromis et que tous les systèmes peuvent devenir indisponibles. De la même manière, les communications concernant un incident doivent être effectuées via des canaux qui ne font pas partie des systèmes de l'organisation ; supposons que les adversaires contrôlent tout et peuvent lire les canaux de communication tels que le courrier électronique.


---


Étape de préparation (partie 2)
===============================

Une autre partie de l' `preparation`étape consiste à se protéger contre les incidents. Bien que la protection ne relève pas nécessairement de la responsabilité d'une équipe de gestion des incidents, toutes les activités liées à la protection doivent lui être connues afin de mieux comprendre le type et la complexité d'un incident et de savoir où chercher des artefacts/preuves qui pourraient faciliter l'enquête.

Examinons maintenant certaines des mesures de protection hautement recommandées, qui ont un impact important sur la majorité des menaces.

* * * * *

DMARC
-----

[DMARC](https://dmarcly.com/blog/how-to-implement-dmarc-dkim-spf-to-stop-email-spoofing-phishing-the-definitive-guide#what-is-dmarc) est une protection de messagerie contre le phishing construite sur le [SPF](https://dmarcly.com/blog/how-to-implement-dmarc-dkim-spf-to-stop-email-spoofing-phishing-the-definitive-guide#what-is-spf) et [le DKIM](https://dmarcly.com/blog/how-to-implement-dmarc-dkim-spf-to-stop-email-spoofing-phishing-the-definitive-guide#what-is-dkim) déjà existants . L'idée derrière DMARC est de rejeter les e-mails qui « prétendent » provenir de votre organisation. Par conséquent, si un adversaire usurpe un e-mail en se faisant passer pour un employé demandant le paiement d'une facture, le système rejettera l'e-mail avant qu'il n'atteigne le destinataire prévu. DMARC est facile et peu coûteux à mettre en œuvre, cependant, je ne saurais trop insister sur le fait que des tests approfondis sont obligatoires ; sinon (et c'est souvent le cas), vous risquez de bloquer les e-mails légitimes sans pouvoir les récupérer.

Grâce aux règles de filtrage des e-mails, vous pourrez peut-être faire passer DMARC au niveau « supérieur » et appliquer une protection supplémentaire contre les e-mails échouant à DMARC provenant de domaines dont vous ne possédez pas. Cela est possible car certains systèmes de messagerie effectuent une vérification DMARC et incluent un en-tête indiquant si DMARC a réussi ou échoué dans les en-têtes de message. Bien que cela puisse être incroyablement puissant pour détecter les e-mails de phishing provenant de n'importe quel domaine, cela nécessite des tests approfondis avant de pouvoir être introduit dans un environnement de production. Les faux positifs élevés ici sont des e-mails envoyés « au nom de » via un service d'envoi d'e-mails, car ils ont tendance à échouer DMARC en raison d'une incompatibilité de domaine.

* * * * *

Renforcement des points finaux (& EDR)
--------------------------------------

Les terminaux (postes de travail, ordinateurs portables, etc.) sont les points d'entrée de la plupart des attaques auxquelles nous sommes confrontés au quotidien. Si l'on considère que la plupart des menaces proviendront d'Internet et cibleront les utilisateurs qui parcourent des sites Web, ouvrent des pièces jointes ou exécutent des exécutables malveillants, un pourcentage de cette activité proviendra de leurs points de terminaison d'entreprise.

Il existe désormais quelques normes de renforcement des points de terminaison largement reconnues, les références CIS et Microsoft étant les plus populaires, et celles-ci devraient réellement constituer les éléments de base des références de renforcement de votre organisation. Certaines actions très importantes (qui fonctionnent réellement) à noter et à faire sont :

-   Désactiver LLMNR/NetBIOS
-   Implémentez LAPS et supprimez les privilèges administratifs des utilisateurs réguliers
-   Désactiver ou configurer PowerShell en mode "ConstrainedLanguage"
-   Activer les règles de réduction de la surface d'attaque (ASR) si vous utilisez Microsoft Defender
-   Mettez en œuvre la liste blanche. Nous savons que c'est presque impossible à mettre en œuvre. Envisagez au moins de bloquer l'exécution à partir des dossiers accessibles en écriture par l'utilisateur (Téléchargements, Bureau, AppData, etc.). Ce sont les emplacements où les exploits et les charges utiles malveillantes se retrouveront initialement. N'oubliez pas de bloquer également les types de scripts tels que .hta, .vbs, .cmd, .bat, .js et similaires. Veuillez faire attention aux fichiers [LOLBin](https://lolbas-project.github.io/) lors de la mise en œuvre de la liste blanche. Ne les négligez pas ; ils sont vraiment utilisés dans la nature comme accès initial pour contourner la liste blanche.
-   Utilisez des pare-feu basés sur l'hôte. Au strict minimum, bloquez la communication entre postes de travail et le trafic sortant vers LOLBins.
-   Déployez un produit EDR. À l'heure actuelle, [AMSI](https://learn.microsoft.com/en-us/windows/win32/amsi/how-amsi-helps) offre une grande visibilité sur les scripts obscurcis permettant aux produits anti-programme malveillant d'inspecter le contenu avant son exécution. Il est fortement recommandé de choisir uniquement des produits qui s'intègrent à AMSI.

Lorsqu'il s'agit de durcissement, `Don't let perfect be the enemy of good`.

* * * * *

Protection du réseau
--------------------

La segmentation du réseau est une technique puissante pour éviter qu'une violation ne se propage à l'ensemble de l'organisation. Les systèmes critiques pour l'entreprise doivent être isolés et les connexions doivent être autorisées uniquement selon les besoins de l'entreprise. Les ressources internes ne devraient vraiment pas faire face directement à Internet (sauf si elles sont placées dans une DMZ).

De plus, lorsque vous parlez de protection du réseau, vous devez considérer les systèmes IDS/IPS. Leur pouvoir brille vraiment lorsque l'interception SSL/TLS est effectuée afin qu'ils puissent identifier le trafic malveillant sur la base du contenu sur le fil et non sur la base de la réputation des adresses IP, ce qui est un moyen traditionnel et très inefficace de détecter le trafic malveillant.

De plus, assurez-vous que seuls les appareils approuvés par l'organisation peuvent accéder au réseau. Des solutions telles que 802.1x peuvent être utilisées pour réduire le risque de connexion de votre propre appareil (BYOD) ou d'appareils malveillants se connectant au réseau d'entreprise. Si vous êtes une entreprise uniquement cloud utilisant, par exemple, Azure/Azure AD, vous pouvez obtenir une protection similaire avec des politiques d'accès conditionnel qui autoriseront l'accès aux ressources de l'organisation uniquement si vous vous connectez à partir d'un appareil géré par l'entreprise.

* * * * *

Gestion des identités privilégiées / MFA / Mots de passe
--------------------------------------------------------

À l'heure actuelle, le vol des informations d'identification d'utilisateurs privilégiés constitue la voie d'escalade la plus courante dans les environnements Active Directory. De plus, une erreur courante est que les utilisateurs administrateurs ont soit un mot de passe faible (mais souvent complexe), soit un mot de passe partagé avec leur compte utilisateur habituel (qui peut être obtenu via plusieurs vecteurs d'attaque tels que l'enregistrement de frappe). Pour référence, un mot de passe faible mais complexe est « Mot de passe 1 ! ». Il comprend des majuscules, des minuscules, des chiffres et des caractères spéciaux, mais malgré cela, il est facilement prévisible et peut être trouvé dans de nombreuses listes de mots de passe que les adversaires utilisent dans leurs attaques. Il est recommandé d'apprendre aux employés à utiliser des phrases secrètes, car elles sont plus difficiles à deviner et difficiles à forcer. Un exemple de phrase de mot de passe facile à retenir mais longue et complexe est « j'aime mon café chaud ». Si l'on connaît une deuxième langue, on peut mélanger des mots de plusieurs langues pour une protection supplémentaire.

L'authentification multifacteur (MFA) est une autre solution de protection de l'identité qui devrait être mise en œuvre au moins pour tout type d'accès administratif à TOUTES les applications et tous les appareils.

* * * * *

Analyse des vulnérabilités
--------------------------

Effectuez des analyses continues des vulnérabilités de votre environnement et corrigez au moins les vulnérabilités « élevées » et « critiques » découvertes. Bien que l'analyse puisse être automatisée, les correctifs nécessitent généralement une intervention manuelle. Si vous ne pouvez pas appliquer de correctifs pour une raison quelconque, segmentez définitivement les systèmes vulnérables !

* * * * *

Formation de sensibilisation des utilisateurs
---------------------------------------------

Former les utilisateurs à reconnaître les comportements suspects et à les signaler lorsqu'ils sont découverts est une grande victoire pour nous. Même s'il est peu probable qu'il soit possible d'atteindre 100 % de réussite dans cette tâche, ces formations sont connues pour réduire considérablement le nombre de compromis réussis. Des tests « surprise » périodiques devraient également faire partie de cette formation, incluant, par exemple, des e-mails de phishing mensuels, des clés USB tombées dans l'immeuble de bureaux, etc.

* * * * *

Évaluation de la sécurité Active Directory
------------------------------------------

La meilleure façon de détecter les erreurs de configuration de sécurité ou les vulnérabilités critiques exposées est de les rechercher du point de vue d'un attaquant. Effectuer vos propres évaluations (ou embaucher un tiers si l'ensemble des compétences manque au sein de l'organisation) garantira que lorsqu'un périphérique de point final est compromis, l'attaquant ne disposera pas d'une possibilité d'escalade en une étape vers des privilèges élevés sur le réseau. Plus un attaquant génère d'outils et d'activités supplémentaires, plus vous avez de chances de les détecter. Essayez donc d'éliminer autant que possible les gains faciles et les fruits à portée de main.

Active Directory dispose de quelques chemins/bogues de remontée d'informations connus et uniques. De nouveaux sont également souvent découverts. Les évaluations de sécurité Active Directory sont cruciales pour la posture de sécurité de l'environnement dans son ensemble. Ne présumez pas que vos administrateurs système sont au courant de tous les bogues découverts ou publiés, car en réalité ce n'est probablement pas le cas.

* * * * *

Exercices de l'équipe violette
------------------------------

Nous devons former les gestionnaires d'incidents et les maintenir engagés. Cela ne fait aucun doute, et le meilleur endroit pour le faire est au sein de l'environnement propre d'une organisation. Les exercices de l'équipe violette sont essentiellement des évaluations de sécurité réalisées par une équipe rouge qui informent en permanence ou éventuellement l'équipe bleue de leurs actions, découvertes, éventuelles lacunes en matière de visibilité/sécurité, etc. De tels exercices aideront à identifier les vulnérabilités d'une organisation tout en testant les capacités défensives de l'équipe bleue. capacité en termes de journalisation, de surveillance, de détection et de réactivité. Si une menace passe inaperçue, il existe une possibilité de s'améliorer. Pour ceux qui sont détectés, l'équipe bleue peut tester tous les playbooks et procédures de gestion des incidents pour s'assurer qu'ils sont robustes et que le résultat attendu a été atteint.