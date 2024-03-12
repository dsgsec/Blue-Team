# Analyse d'en-tête d'e-mail

Dans les sections précédentes, nous avons expliqué ce qu'est un e-mail de phishing, ce que sont les informations d'en-tête et ce qu'elles font. Désormais, lorsque nous soupçonnons qu'un e-mail est du phishing, nous saurons ce que nous devons faire et à quoi doit ressembler le processus d'analyse.

Voici les questions clés auxquelles nous devons répondre lors de la vérification des titres lors d'une analyse de phishing :

-   L'e-mail a-t-il été envoyé depuis le bon serveur SMTP ?-   Les données « From » et « Return-Path / Reply-To » sont-elles identiques ?

L'e-mail examiné dans la suite de l'article :

mot de passe : infecté\
[Télécharger](https://drive.google.com/file/d/1x4BQF9zdR2l913elSQtixb-kmi9Jan_6/view?usp=sharing)

L'e-mail a-t-il été envoyé depuis le bon serveur SMTP ?

On peut vérifier le champ "Reçu" pour voir le chemin suivi par le mail. Comme le montre l'image ci-dessous, le mail est "101[.]99.94.116" du serveur d'adresse IP.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/Phishing+Email+Analysis/received-header.PNG)

Si l'on regarde qui envoie le mail ("expéditeur"), on voit qu'il provient du domaine Letsdefend.io

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/Phishing+Email+Analysis/email-from.PNG)

Ainsi, dans des circonstances normales, "letsdefend.io" devrait utiliser "101[.]99.94.116" pour envoyer du courrier. Pour confirmer cette situation, nous pouvons interroger les serveurs MX activement utilisés par "letsdefend.io"

"mxtoolbox.com" vous aide en vous montrant les serveurs MX utilisés par le domaine que vous avez recherché.

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/Phishing+Email+Analysis/mxtoolbox.PNG)

Si l'on regarde l'image ci-dessus, le domaine "letsdefend.io" utilise les adresses Google comme serveur de messagerie. Il n'y a donc aucun rapport avec les adresses emkei[.]cz ou "101[.]99.94.116".

Lors de cette vérification, il a été déterminé que l'e-mail ne provenait pas de l'adresse d'origine, mais qu'il avait été usurpé.

Les données « From » et « Return-Path / Reply-To » sont-elles identiques ?

Sauf cas exceptionnels, nous attendons que l'expéditeur de l'e-mail et la personne recevant les réponses soient les mêmes. Un exemple de la raison pour laquelle ces zones sont utilisées différemment dans les attaques de phishing :

Quelqu'un envoie un e-mail (gmail, hotmail, etc.) avec le même nom de famille qu'une personne travaillant pour Google à LetsDefend, LetsDefend informe l'employé qu'il a émis la facture et qu'il doit effectuer le paiement sur son compte XXX. Il met l'adresse e-mail du véritable employé de Google dans le champ "Répondre à" afin que la fausse adresse e-mail ne ressorte pas en cas de réponse à un éventuel e-mail.

En revenant à l'e-mail que nous avons téléchargé ci-dessus, il suffit de comparer les adresses e-mail dans les champs « De » et « Répondre à ».

![](https://letsdefend-images.s3.us-east-2.amazonaws.com/Courses/Phishing+Email+Analysis/reply-to.PNG)

Comme vous pouvez le constater, les données sont différentes. En d'autres termes, lorsque nous souhaitons répondre à cet e-mail, nous enverrons une réponse à l'adresse Gmail ci-dessous. Ce n'est pas parce que ces données sont différentes qu'il s'agit définitivement d'un e-mail de phishing ; nous devons considérer l'événement dans son ensemble. En d'autres termes, en plus de cette situation suspecte, s'il y a une pièce jointe nuisible, une URL ou un contenu trompeur dans le contenu de l'e-mail, nous pouvons comprendre que l'e-mail est du phishing. Dans la suite de la formation, nous analyserons les données contenues dans la partie corps de l'e-mail.
