### Usurpation

Les attaquants peuvent envoyer des e-mails au nom de quelqu'un d'autre, car les e-mails ne disposent pas nécessairement d'un mécanisme d'authentification. Les attaquants peuvent envoyer du courrier au nom de quelqu'un d'autre en utilisant la technique appelée usurpation d'identité pour faire croire à l'utilisateur que le courrier électronique entrant est fiable. Plusieurs protocoles ont été créés pour empêcher la technique d'Email Spoofing. Grâce aux protocoles SPF, DKIM et DMARC, il est possible de comprendre si l'adresse de l'expéditeur est fausse ou réelle. Certaines applications de messagerie effectuent ces vérifications automatiquement. Cependant, l'utilisation de ces protocoles n'est pas obligatoire et peut dans certains cas poser des problèmes.

-   Cadre de politique de l'expéditeur (SPF)
-   Courrier identifié par DomainKeys (DKIM)

Pour savoir manuellement si le courrier est frauduleux ou non, l'adresse SMTP du courrier doit d'abord être apprise. Les enregistrements SPF, DKIM, DMARC et MX du domaine peuvent être appris à l'aide d'outils tels que [Mxtoolbox](https://mxtoolbox.com/) . En comparant les informations ici, il est possible de savoir si le courrier est frauduleux ou non.

![](https://umuttosun.com/wp-content/uploads/2020/05/Screen-Shot-2020-05-10-at-15.21.06-1-1024x291.png)

Étant donné que les adresses IP des grandes institutions utilisant leurs propres serveurs de messagerie leur appartiendront, il peut être vérifié si l'adresse SMTP appartient à cette institution en examinant les enregistrements whois de l'adresse IP SMTP.

Un point important ici est que si l'adresse de l'expéditeur n'est pas usurpée, nous ne pouvons pas dire que le courrier est sûr. Des courriers nuisibles peuvent être envoyés au nom de personnes de confiance en piratant des adresses e-mail professionnelles/personnelles. Ce type de cyberattaques s'est déjà produit, cette possibilité doit donc toujours être envisagée.

### Analyse du trafic de messagerie

De nombreux paramètres sont nécessaires lors de l'analyse d'une attaque de phishing. Nous pouvons connaître l'ampleur de l'attaque et le public cible dans les résultats de recherche à effectuer sur la passerelle de messagerie en fonction des paramètres suivants.

-   Adresse de l'expéditeur (info@letsdefend.io)
-   Adresse IP SMTP (127.0.0.1)
-   @letsdefend.io (base de domaine)
-   Letsdefend (Outre le compte Gmail, l'attaquant peut avoir envoyé depuis le compte Hotmail)
-   Objet (l'adresse de l'expéditeur et l'adresse SMTP peuvent changer constamment)

Dans les résultats de la recherche, il est nécessaire de connaître les adresses des destinataires et les informations horaires en plus des numéros de courrier. Si des e-mails nuisibles sont constamment transmis aux mêmes utilisateurs, leurs adresses e-mail peuvent avoir été divulguées d'une manière ou d'une autre et partagées sur des sites tels que PasteBin.

Les attaquants peuvent trouver des adresses e-mail avec l'outil Harvester sur Kali Linux. Il est recommandé que ces informations ne soient pas partagées explicitement, car la conservation d'adresses e-mail personnelles sur des sites Web constituerait un vecteur d'attaque potentiel pour les attaquants.
