# Analyse statique
C'est un fait que les mails composés de texte brut sont ennuyeux. Pour cette raison, les applications de messagerie fournissent un support HTML, permettant la création de courriers électroniques susceptibles d'attirer davantage l'attention des utilisateurs. Bien entendu, cette fonctionnalité présente un inconvénient. Les attaquants peuvent créer des e-mails avec HTML, cachant les adresses URL nuisibles derrière des boutons/textes qui semblent inoffensifs.

![](https://letsdefend.io/blog/wp-content/uploads/2020/08/Screen-Shot-2020-08-22-at-10.45.13-1024x242.png)

Comme le montre l'image ci-dessus, l'adresse que l'utilisateur voit peut être différente lorsqu'il clique sur le lien (la véritable adresse est vue lorsque le lien est survolé).

Dans la plupart des attaques de phishing, les attaquants prennent une nouvelle adresse de domaine, lancent une attaque de phishing en quelques jours et terminent leur travail. Pour cette raison, si le nom de domaine dans le courrier est nouveau, il est plus probable qu'il s'agisse d'une attaque de phishing.

[Twitter](https://twitter.com/intent/tweet?text=Attackers%20take%20a%20new%20domain%20address%20in%20most%20phishing%20attacks%20and%20do%20a%20phishing%20attack%20within%20a%20few%20days%20and%20finish%20their%20work.%20For%20this%20reason,%20if%20the%20domain%20name%20in%20the%20mail%20is%20new,%20it%20is%20more%20likely%20to%20be%20a%20phishing%20attack.%20@LetsDefendIO%20https://app.letsdefend.io/academy/lesson/Static-Analysis/)[LinkedIn](https://www.linkedin.com/shareArticle?mini=true&url=https://app.letsdefend.io/academy/lesson/Static-Analysis/&title=Blue%20Team%20Training)[Facebook](https://www.facebook.com/sharer/sharer.php?u=https://app.letsdefend.io/academy/lesson/Static-Analysis/)

Il est possible de savoir si les moteurs antivirus détectent l'adresse Web comme dangereuse en recherchant les adresses Web dans le courrier sur VirusTotal. Si quelqu'un d'autre a déjà analysé la même adresse/fichier dans VirusTotal, VirusTotal n'analyse pas à partir de zéro, il vous montre l'ancien résultat de l'analyse. Nous pouvons utiliser cette fonctionnalité à la fois comme un avantage et un inconvénient.

![](https://umuttosun.com/wp-content/uploads/2020/05/Screen-Shot-2020-05-04-at-05.07.11-1024x156.png)

Si l'attaquant recherche l'adresse de domaine sur VirusTotal sans contenir de contenu nuisible, cette adresse apparaîtra inoffensive sur VirusTotal, et si elle passe inaperçue, vous pourriez croire à tort que cette adresse est inoffensive. Dans l'image ci-dessus, vous pouvez voir que l'adresse umuttosun.com semble inoffensive, mais si vous regardez la section marquée de la flèche rouge, vous verrez que cette adresse a été recherchée il y a 9 mois, et ce résultat date d'il y a 9 mois. Pour le faire analyser à nouveau, il faut appuyer sur le bouton marqué de la flèche bleue.

Si la page a déjà été recherchée sur VirusTotal, cela peut signifier que l'attaquant voulait voir le taux de détection du site pendant la phase de préparation. Si nous l'analysons à nouveau, le moteur antivirus le détecte comme du phishing, ce qui signifie que l'attaquant tente de tromper les analystes.

Effectuer une analyse statique des fichiers contenus dans le courrier peut permettre d'apprendre la capacité/les capacités de ce fichier. Cependant, comme l'analyse statique prend beaucoup de temps, vous pouvez obtenir les informations dont vous avez besoin plus rapidement grâce à l'analyse dynamique.

[Cisco Talos Intelligence](https://talosintelligence.com/) comporte des sections de recherche dans lesquelles nous pouvons connaître la réputation des adresses IP. En recherchant l'adresse SMTP du courrier que nous avons détecté sur Talos, nous pouvons voir la réputation de l'adresse IP et savoir si elle est incluse dans la liste noire. Si l'adresse SMTP figure dans la liste noire, on peut comprendre qu'une attaque a été menée sur un serveur compromis.

![](https://umuttosun.com/wp-content/uploads/2020/05/Screen-Shot-2020-05-10-at-15.26.00-1-1024x453.png)

De même, l'adresse SMTP peut être recherchée sur VirusTotal et AbuseIPDB pour déterminer si l'adresse IP a déjà été impliquée dans des activités malveillantes.
