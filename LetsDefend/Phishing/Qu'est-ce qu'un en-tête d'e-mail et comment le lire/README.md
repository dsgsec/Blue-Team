Dans cette section, nous expliquerons ce que sont les informations d'en-tête d'un e-mail, ce qui peut être fait avec ces informations et comment accéder à ces informations. Il est important de suivre attentivement cette section car nous expliquerons comment effectuer l'analyse d'en-tête dans la section suivante.

### Qu'est-ce qu'un en-tête d'e-mail ?

"En-tête" est essentiellement une section du courrier qui contient des informations telles que l'expéditeur, le destinataire et la date. De plus, il existe des champs tels que « Return-Path », « Reply-To » et « Received ». Ci-dessous, vous pouvez voir les détails de l'en-tête d'un exemple d'e-mail.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/Phishing+Email+Analysis/email-header.PNG)

### À quoi sert l'en-tête d'e-mail ?\
Permet l'identification de l'expéditeur et du destinataire

Grâce aux champs « De » et « À » dans l'en-tête, il est déterminé de qui un email sera envoyé à qui. Si nous regardons l'email ci-dessus que vous avez téléchargé au format "eml", nous voyons qu'il a été envoyé depuis l'adresse "ogunal@letsdefend.io" vers "info@letsdefend.io".

![](https://letsdefend.io/blog/wp-content/uploads/2022/03/sample-subject.png)

Bloqueur de spam

Il est possible de détecter les courriers indésirables à l'aide de l'analyse d'en-tête et d'autres méthodes diverses. Cela protège les gens contre la réception de courriers indésirables.

Permet de suivre l'itinéraire d'un e-mail

Il est important de vérifier le parcours qu'il suit pour voir si un email provient de la bonne adresse. Si nous regardons l'exemple d'e-mail ci-dessus, nous voyons qu'il provient de l'adresse « ogunal@letsdefend.io », mais provient-il réellement du domaine «letsdefend.io» ou d'un autre faux serveur qui imite le même nom ? Nous pouvons utiliser les informations d'en-tête pour répondre à cette question.

### Champs importants
**De**

Le champ « De » dans l'en-tête Internet indique le nom et l'adresse e-mail de l'expéditeur.

**À**

Ce champ dans l'en-tête du courrier contient les détails du destinataire du courrier électronique.

Il comprend leur nom et leur adresse e-mail. Les champs tels que CC (copie carbone) et BCC (copie carbone invisible) entrent également dans cette catégorie car ils incluent tous les détails de vos destinataires.

Si vous souhaitez en savoir plus sur la copie carbone et la copie carbone invisible, découvrez comment utiliser CC et BCC.

**Date**

Il s'agit de l'horodatage qui indique la date à laquelle l'e-mail a été envoyé.

Dans Gmail, il suit généralement le format « jour jj mois aaaa hh: mmss ».

Ainsi, si un e-mail avait été envoyé le 16 novembre 2021 à 16 h 57 min 23 s, il s'afficherait comme le mercredi 16 novembre 2021. 16 :57:23.

**Objet**

Le sujet mentionne le sujet de l'e-mail. Il résume le contenu de l'intégralité du corps du message.

**Return-Path**

Ce champ d'en-tête de courrier est également appelé Répondre à. Si vous répondez à un e-mail, il sera envoyé à l'adresse mentionnée dans le champ Chemin de retour.

Clé de domaine et signatures DKIM La clé de domaine et le courrier identifié par clé de domaine (DKIM) sont des signatures de courrier électronique qui aident les fournisseurs de services de messagerie à identifier et authentifier vos courriers électroniques, similaires aux signatures

SPF. Le champ d'en-tête de l'ID de message est une combinaison unique de lettres et de chiffres qui identifient chaque courrier. Deux e-mails n'auront pas le même ID de message. MIME-Version MultiPurpose Internet Mail Extensions (MIME) est une norme Internet de codage. Il convertit le contenu non textuel. comme des images, des vidéos et d'autres pièces jointes en texte afin qu'elles puissent être jointes à un e-mail et envoyées via SMTP (Simple Mail Transfer Protocol). Reçu Le champ Reçu répertorie chaque serveur de messagerie qui a traité un e-mail avant d'arriver dans la boîte de réception du destinataire. Il est répertorié dans l'ordre chronologique inverse : le serveur de messagerie en haut est le dernier serveur par lequel le message électronique est passé et le serveur du bas est celui d'où provient le courrier électronique. Statut X-Spam

Le statut X-Spam vous montre le score de spam d'un e-mail.\
Tout d'abord, il indiquera si un message est classé comme spam.\
Ensuite, le score de spam de l'e-mail est affiché, ainsi que le seuil de spam pour l'e-mail.\
Un e-mail peut soit atteindre le seuil de spam d'une boîte de réception, soit le dépasser. S'il contient trop de spam et dépasse le seuil, il sera automatiquement classé comme spam et envoyé dans le dossier spam.

*Définitions des champs : gmass.co*

### Comment accéder à l'en-tête de votre e-mail ?\
Gmail

1- Ouvrez l'e-mail concerné\
2- Cliquez sur les 3 points en haut à droite "..."\
3- Cliquez sur le bouton "Télécharger le message".

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/Phishing+Email+Analysis/mail-header-gmail.PNG)\
4- Téléchargé ".Ouvrez le fichier avec l'extension "eml" avec n'importe quelle application notebook

Outlook

1- Ouvrez l'e-mail correspondant\
2- Fichier -> Info -> Propriétés -> En-têtes Internet

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/Phishing+Email+Analysis/outlook-1.png)

![outlook2](https://github.com/dsgsec/Blue-Team/assets/82456829/88421684-26a8-439f-b16d-a27ef75d05b4)
