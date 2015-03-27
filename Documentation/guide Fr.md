#Apprendre Angular en un jour, le guide ultime

##Qu'est-ce qu'AngularJS ?

Angular est un framework MVC/MVVM côté client, développé en JavaScript, ce qui est obligatoire pour créer une application moderne à page unique (ou même un site internet). 

C'est un grand bond vers le futur de HTML et vers ce que HTML5 apporte. 

C'est également un grand bol d'air frais dans le monde des applications web modernes. 

Cet article est une vue de bout en bout, issue de mon expérience, et contient des conseils et astuces glanés au travers de mon utilisation d'Angular.

##Terminologie

Angular a une courbe d'apprentissage assez courte qui consiste principalement à appréhender la terminologie et la “pensée MVC”. 

MVC signifie Modèle-Vue-Contrôleur. 
Survolons un peu les différents composants et voyons un peu la terminologie en jetant un oeil sur les APIs essentielles d'Angular.

###MVC

Vous avez probablement déjà entendu parler de MVC. Utilisé dans de nombreux langages de programmation pour apporter une structure/architecture à une application. En voici les composants :

####Modèle

Structure de données représentant une entité de l'application, généralement transmise en JSON. 
Pour bien démarrer avec Angular, quelques notions de JSON sont nécessaires, cela vous permettra de faire communiquer votre serveur et vos vues.
Un groupe d’id d'utilisateurs pourrait par exemple ressembler à ceci :
````
{
  "users" : [{
    "name": "Joe Bloggs",
    "id": "82047392"
  },{
    "name": "John Doe",
    "id": "65198013"
  }]
}
````
Vous pouvez accéder à cette information de deux façons. En passant par une XHR (XMLHttp Request), vous connaissez sûrement $.ajax en jQuery, Angular l'encapsule dans $http. 

L'autre méthode est de l'écrire dans le code de la page pour qu'elle soit chargée pendant l'interprétation (depuis un datastore ou une base de données). 

Une fois que vous avez accès à l'information, vous pouvez mettre à jour votre modèle et le renvoyer.

####Vue

La vue est simple, c'est votre HTML et/ou la sortie générée. Lorsque vous utilisez un framework MVC, vous utilisez les données issues du Modèle pour mettre votre Vue à jour et afficher les bonnes informations dans votre HTML.

####Contrôleur

Comme son nom l'indique, cette couche contrôle des choses. 
Mais quelles choses ? Des données. 
Les contrôleurs permettent à votre serveur de communiquer avec la Vue, c'est le messager, vous pouvez donc mettre vos données à jour à la volée via ces canaux de communication entre le serveur et le client.
