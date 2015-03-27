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

###Mettre en place un projet AngularJS (l'essentiel)

Tout d'abord, nous devons mettre en place le minimum vital d'un projet Angular. 

Nous devons mettre en place certaines choses avant de commencer. 

Cela revient, en général, à ajouter une déclaration ng-app, écrire un contrôleur pour parler à la vue puis l'inclusion d'Angular et un attachement au DOM. Voici l'essentiel :

Un peu de HTML avec les déclarations `ng-*` :
````
<div ng-app="myApp">
  <div ng-controller="MainCtrl">
    <!-- logique du contrôleur -->
  </div>
</div>
````

Un module Angular et un contrôleur :
````
var myApp = angular.module('myApp', []);
myApp.controller('MainCtrl', ['$scope', function ($scope) {
  // Magie du contrôleur
}]);
````

Avant de nous lancer, nous devons créer un module Angular dans lequel nous allons placer toute notre logique. 
Il existe plusieurs manières de déclarer des modules et vous pouvez chaîner toute votre logique (je n'aime pas cette méthode) :
````
angular.module('myApp', [])
.controller('MainCtrl', ['$scope', function ($scope) {...}])
.controller('NavCtrl', ['$scope', function ($scope) {...}])
.controller('UserCtrl', ['$scope', function ($scope) {...}]);
````

La mise en place un module global s'est révélée être la meilleure pratique sur les projets Angular sur lesquels j'ai travaillé. 
L'absence de point-virgules ou les fermetures accidentelles de chaîne se sont montrées contre productives et ont souvent généré des erreurs de compilation. 
Préférez cette approche :
````
var myApp = angular.module('myApp', []);
myApp.controller('MainCtrl', ['$scope', function ($scope) {...}]);
myApp.controller('NavCtrl', ['$scope', function ($scope) {...}]);
myApp.controller('UserCtrl', ['$scope', function ($scope) {...}]);
````
Chaque nouveau fichier que je crée utilise le namespace myApp ce qui le restreint à l'application. 
Oui, je crée un nouveau fichier pour chaque Contrôleur, Directive, Factory ou tout autre élément (vous me remercierez plus tard). Joignez-les dans un fichier à la volée en utilisant Grunt ou un outil similaire.

###Contrôleurs

Maintenant que vous avez une idée de ce qu'est le MVC et que tout est en place, jetons un oeil à la façon dont Angular implémente les contrôleurs.

Reprenons l'exemple vu plus haut et regardons pas à pas comment pousser des données dans le DOM depuis un contrôleur. 
Angular utilise un système de template qui ressemble à ceci pour parler à votre HTML : `{{ handlebars }}`. 
Idéalement, votre HTML ne contient aucun texte ou valeur en dur, cela permet de tirer un maximum d'Angular. 

Voici un exemple dans lequel nous poussons une chaîne de caractères dans le DOM :
````
<div ng-app="myApp">
  <div ng-controller="MainCtrl">
    {{ text }}
  </div>
</div>
````
````
var myApp = angular.module('myApp', []);

myApp.controller('MainCtrl', ['$scope', function ($scope) {

    $scope.text = 'Hello, Angular fanatic.';

}]);
````

Le concept clé ici est `$scope` que vous passez à toutes vos fonctions au sein d'un contrôleur. 

`$scope` fait référence à l'élément courant (ou zone courante) dans le DOM (ce qui est différent de this). 

Il encapsule intelligemment les données et la logique pour que celles-ci soient limitées à l'élément. 

Cela apporte une notion de publique/privé à JavaScript, ce qui est fantastique.

Le concept de `$scope` peut faire peur de prime abord mais c'est votre canal de communication avec le DOM depuis le serveur (ou depuis les données statiques si vous en avez). La démo donne une petite idée de comment “pousser” des données dans le DOM.

Regardons maintenant une structure de données plus représentative que nous avons hypothétiquement récupérée depuis le serveur pour afficher les détails de l'utilisateur. À partir de maintenant, j'utiliserai des données statiques, je vous montrerai plus tard comment récupérer dynamiquement des données JSON.

Commençons par un peu de JavaScript :

````
var myApp = angular.module('myApp', []);

myApp.controller('UserCtrl', ['$scope', function ($scope) {
    // Créons un namespace pour les détails de l'utilisateur
    // Également utile pour une aide visuelle dans le DOM
    $scope.user = {};
    $scope.user.details = {
      "username": "Todd Motto",
      "id": "89101112"
    };

}]);
````

Poussons ces données vers le DOM pour les afficher :
````
<div ng-app="myApp">
  <div ng-controller="UserCtrl">
    <p class="username">Welcome, {{ user.details.username }}</p>
    <p class="id">User ID: {{ user.details.id }}</p>
  </div>
</div>
````

Il est important de noter que les contrôleurs ont pour but de gérer les données et de contenir des fonctions (ou événements) qui parlent au serveur pour envoyer/recevoir des données JSON. 

Aucune manipulation du DOM ne doit y prendre place, pas de jQuery ici donc. 

La manipulation du DOM se fait par les directives que nous allons voir ensuite.

Astuce : dans la documentation d'Angular (au moment de la rédaction de cet article) leurs exemples créent un contrôleur comme ceci :
````
var myApp = angular.module('myApp', []);

function MainCtrl ($scope) {
  //...
};
````

… N'en faites rien. 

Cela expose toutes vos fonctions au contexte global et ne les cantonne pas à votre application. 

Cela signifie également que vous ne pouvez pas minifier votre code ou le tester facilement. Ne remplissez pas le namespace global et gardez vos contrôleurs dans votre application.


###Directives

Une directive (lisez mon article sur les directives issues de scripts/plugins existants [en]), dans sa forme la plus simple, est un petit morceau de template HTML, utilisé de préférence à plusieurs endroits de l'application. 

C'est un moyen facile d'injecter sans effort du DOM dans votre application ou d'effectuer des interactions particulières avec le DOM. 

Les directives ne sont pas simples pour autant et la courbe d'apprentissage pour les maitriser est assez importante. Ce qui suit devrait tout de même vous donner un bon point de départ.

À quoi servent donc les directives ?

Beaucoup de choses dont la création de composants DOM (onglets ou éléments de navigation) - tout dépend de l'usage que votre application fait de l'interface utilisateur. 

Si vous avez, par exemple, joué un peu avec `ng-show` ou `ng-hide`, ce sont des directives (qui n'injectent pas de DOM).

Pour cet exercice, je vais faire simple et créer un type de bouton personnalisé (appelé `customButton`) qui injecte quelques balises que je déteste devoir écrire partout. 

Il y a plusieurs façons de définir une directive dans le DOM. Voici quelques exemples :
````
<!-- 1: déclaration en tant qu'attribut -->
<a custom-button>Click me</a>

<!-- 2: en tant que nouvel élément -->
<custom-button>Click me</custom-button>

<!-- 3: en tant que classe (pour être compatible avec les vieux IE) -->
<a class="custom-button">Click me</a>

<!-- 4: en tant que commentaire (peu adapté à cette démo) -->
<!-- directive: custom-button -->
````

Je préfère les utiliser sous forme d'attribut. Les éléments personnalisés feront partie de futures versions de HTML5 sous le nom de Web Components mais Angular les considère comme assez instables, en particulier pour les vieux navigateurs.

Vous savez maintenant comment utiliser une directive, créons maintenant notre bouton. Encore une fois, j'utilise le namespace `myApp`. 

Voici une directive dans sa forme la plus simple :
````
myApp.directive('customButton', function () {
  return {
    link: function (scope, element, attrs) {
      // manipulation du DOM et événements
    }
  };
});
````

Je définis ma directive avec la méthode `.directive()` en lui passant le nom de la directive `'customButton'`. 

Lorsque vous capitalisez une lettre dans le nom d'une directive, il faut utiliser un tiret lorsque vous vous en servez (comme dans l'exemple vu plus haut).

Une directive retourne une référence vers elle-même via un Object et accepte un certain nombre de paramètres. 

Selon moi, les plus importants à connaitre sont `restrict`, `replace`, `transclude`, `template`, `templateUrl` et, bien sûr, la propriété `link`. 

Ajoutons-les :
````
myApp.directive('customButton', function () {
  return {
    restrict: 'A',
    replace: true,
    transclude: true,
    template: '<a href="" class="myawesomebutton" ng-transclude>' +
                '<i class="icon-ok-sign"></i>' +
              '</a>',
    link: function (scope, element, attrs) {
      // DOM manipulation/events here!
    }
  };
});
````

Utilisez Inspecter l'Élément pour vérifier que les balises ont bien été injectées. 

Je sais, il n'y a pas d'icône parce que je n'ai pas inclus Font Awesome, mais vous voyez le principe. 

En ce qui concerne les propriétés d'une directive :

####restrict

Indique la méthode d'accès à l'élément. 

Si votre projet doit être compatible avec d'anciennes versions d'IE, préférez une déclaration de type attribut ou classe.

Indiquer `'A'` signifie attribut, `'E'` signifie élément, `'C'` signifie classe et `'M'` signifie commentaire. 

La valeur par défaut est `'EA'`. 

Oui, il est tout à fait possible d'utiliser plusieurs méthodes d'accès en même temps.


####replace

Indique si le balisage appelant la directive doit être remplacé ou non. 

Dans l'exemple précédent, vous pouvez voir que le DOM initial est remplacé par le template de la directive.

####transclude

Indique si le DOM original doit être copié dans la directive. 

Dans notre exemple, le texte ‘Click me’ apparait dans la directive lorsque celle-ci est affichée.

####template

Un template, comme ci-dessus, permet de déclarer le balisage à injecter.

Il est préférable d'utiliser cette propriété pour de petits bouts de HTML uniquement. 

Les templates injectés sont compilés par Angular, vous pouvez donc utiliser des balises handlebars et les bindings dans ceux-ci.

####templateUrl

Similaire à `template`, permet de garder les templates dans leurs propres fichiers ou au sein de balises `<script>`. 

Cette propriété permet d'indiquer l'URL du template à utiliser. 

Pour des morceaux de HTML un peu plus complexes, il est préférable de les placer chacun dans un fichier spécifique, dans un dossier templates :
````
myApp.directive('customButton', function () {
  return {
    templateUrl: 'templates/customButton.html'
    // directive stuff...
  };
});
````

Et dans votre fichier (nom insensible à la casse) :

````
<!-- dans customButton.html -->
<a href="" class="myawesomebutton" ng-transclude>
  <i class="icon-ok-sign"></i>
</a>
````
