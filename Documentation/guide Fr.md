http://www.tinci.fr/blog/apprendre-angular-en-un-jour-le-guide-ultime/
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

L'intérêt de cette technique est que le fichier HTML va être mis en cache par le navigateur. Une alternative, qui n'est pas mise en cache, est de déclarer le template dans une balise `<script> `:
````
<script type="text/ng-template" id="customButton.html">
<a href="" class="myawesomebutton" ng-transclude>
  <i class="icon-ok-sign"></i>
</a>
</script>
````

Cela indique à Angular que c'est un ng-template et lui donne un ID. 

Angular va chercher le `ng-template` ou le fichier *.hml. Je préfère utiliser des fichiers *.html, ils sont plus faciles à gérer, augmentent les performances et gardent le DOM propre. Si vous avez une centaine de directives, ce sera plus simple pour les parcourir.

###Services

Les services sont souvent une notion un peu floue. 

D'après mes lectures et ma propre expérience, les services sont plus un design pattern de style qu'un réel apport de fonctionnalité. J'ai lu le code source d'Angular et a priori ils sont très proches des factories. 

Ils passent par le même compilateur et semble avoir de nombreuses fonctionnalités en commun. 

Il semble que les services soient préférables pour singleton et les factories pour les fonctions plus complexes comme les Object Literals ou d'autres cas plus compliqués.

Voici un exemple de service qui multiplie deux nombres :
````
myApp.service('Math', function () {
  this.multiply = function (x, y) {
    return x * y;
  };
});
````

Il s'utilise ensuite comme ceci dans un contrôleur :
````
myApp.controller('MainCtrl', ['$scope', function ($scope) {
    var a = 12;
    var b = 24;

    // outputs 288
    var result = Math.multiply(a, b);
}]);
````

Je sais, une multiplication ne nécessite pas un service en soi mais vous voyez où je veux en venir.

Lorsque l'on crée un service (ou une factory) il faut utiliser l'injection de dépendance pour indiquer à Angular de le prendre en charge. Sans cela, on aura une erreur de compilation et notre contrôleur plantera. 

Vous avez sans doute remarqué la partie `function ($scope)` du contrôleur, c'est une simple injection de dépendance, c'est ici que le code doit être placé. Vous aurez remarqué également le `['$scope']` placé avant, j'y reviendrai plus tard. 

Voici comment utiliser une injection de dépendance pour dire à Angular que vous voulez utiliser un service :
````
// Passez Math
myApp.controller('MainCtrl', ['$scope', 'Math', function ($scope, Math) {
    var a = 12;
    var b = 24;

    // outputs 288
    var result = Math.multiply(a, b);
}]);
````

###Factories

Passer des services aux factories devrait être assez simple, on pourrait créer un Object Literal dans une factory ou simplement fournir des méthodes plus avancées :
````
myApp.factory('Server', function () {
  return {
    get: function(url) {
      return $http.get(url);
    },
    post: function(url) {
      return $http.post(url);
    },
  };
});
````

Je crée ici un wrapper personnalisé pour l'objet XHR de Angular. Après injection de dépendance dans le contrôleur, l'utilisation est aisée :
````
myApp.controller('MainCtrl', ['$scope', 'Server', function ($scope, Server) {
    var jsonGet = 'http://myserver/getURL';
    var jsonPost = 'http://myserver/postURL';
    Server.get(jsonGet);
    Server.post(jsonPost);
}]);
````

Si vous vouliez surveiller des changements côté serveur, vous pourriez mettre en place `Server.poll(jsonPoll)` ou, si vous utilisez par exemple un socket, vous pourriez mettre en place `Server.socket(jsonSocket)`. Ce mécanisme permet de modulariser le code et de créer des outils réutilisables tout en gardant le code des contrôleurs à son minimum.

###Filtres

Les filtres sont utiles avec les tableaux de données mais également en dehors des boucles. 

Si vous parcourez une collection de données et que vous souhaitez les filtrer, vous êtes au bon endroit. 

Les filtres peuvent également s'utiliser pour filtrer la saisie d'un utilisateur dans un champ `<input>` par exemple. Les filtres s'utilisent de deux façons : dans un contrôleur ou sous forme de méthode. Voici la version en méthode, qui peut s'utiliser partout :
````
myApp.filter('reverse', function () {
  return function (input, uppercase) {
    var out = '';
    for (var i = 0; i < input.length; i++) {
      out = input.charAt(i) + out;
    }
    if (uppercase) {
      out = out.toUpperCase();
    }
    return out;
  }
});

// Contrôleur inclus pour fournir des données
myApp.controller('MainCtrl', ['$scope', function ($scope) {
    $scope.greeting = 'Todd Motto';
}]);
````

####DOM usage:
````
<div ng-app="myApp">
  <div ng-controller="MainCtrl">
    <p>Sans filtre: {{ greeting }}</p>
    <p>Reverse: {{ greeting | reverse }}</p>
  </div>
</div>
````

Et voici comment l'utiliser dans un ng-repeat :
````
<ul>
  <li ng-repeat="number in myNumbers |filter:oddNumbers">{{ number }}</li>
</ul>
````

Voici également un cas d'utilisation réel de filtres dans un contrôleur :
````
myApp.controller('MainCtrl', ['$scope', function ($scope) {

  $scope.numbers = [10, 25, 35, 45, 60, 80, 100];

  $scope.lowerBound = 42;

  // Does the Filters
  $scope.greaterThanNum = function (item) {
      return item > $scope.lowerBound;
  };

}]);
````

Et son utilisation dans `ng-repeat` :
````
<li ng-repeat="number in numbers | filter:greaterThanNum">
  {{ number }}
</li>
````

Voilà notre tour des briques majeures d'Angular terminé. Ce n'est que le tout début de notre plongée en eaux profondes, mais c'est amplement suffisant pour vous permettre de créer votre première application Angular.

##Binding de données à deux sens

La première fois que j'ai entendu parler de binding de données à deux sens, je n'étais pas sûr de bien comprendre. La meilleure façon de le décrire est sous la forme d'un cercle de données synchronisées : si le modèle est mis à jour, la vue est mise à jour automatiquement ; si la vue est mise à jour, le modèle est automatiquement mis à jour. 

Cela veut dire que sans rien faire, la donnée est synchronisée. Si je bind un `ng-model` à un `<input>` et commence à taper dans ce dernier, cela crée un modèle (ou met à jour un modèle existant) en même temps.

Je crée ici le `<input>` et lui bind un modèle `myModel`, je peux ensuite utiliser les accolades pour afficher la donnée de mon modèle (ainsi que ses mises à jour) dans la vue :
````
<div ng-app="myApp">
  <div ng-controller="MainCtrl">
    <input type="text" ng-model="myModel" placeholder="Start typing..." />
    <p>My model data: {{ myModel }}</p>
  </div>
</div>
````
````
myApp.controller('MainCtrl', ['$scope', function ($scope) {
  // On capture la donnée du modèle
  // et/ou on l'initialise avec une chaîne de caractères vide
  $scope.myModel = '';
}]);
````

## Appels XHR/Ajax/$http et binding JSON

Vous savez maintenant comment envoyer des données à `$scope` et comment fonctionne le binding de données dans les modèles. Il est donc maintenant temps de simuler quelques XHR vers le serveur. 

Ce n'est pas essentiel pour un site web classique, à moins d'avoir des besoins Ajax spécifiques, c'est donc plus orienté vers la récupération de données pour une application web.

En développement local, vous utilisez probablement Java, ASP .NET, PHP ou une autre techno pour faire tourner l'application.

Que vous contactiez une bonne de données locale ou que vous utilisiez ce serveur comme une API pour communiquer avec une autre ressource, la mise en place est globalement la même.

C'est ici que 'dollar http’ entre en scène. 

C'est dorénavant votre meilleur ami. La méthode $http d'Angular est un wrapper bien pratique pour accéder aux données du serveur et est d'une utilisation très simple. Voici un petit exemple pour une requête GET qui, comme vous l'aurez deviné, récupère des données depuis le serveur. 

Sa syntaxe est très proche de celle de jQuery, la transition est donc aisée :
````
myApp.controller('MainCtrl', ['$scope', function ($scope) {
  $http({
    method: 'GET',
    url: '//localhost:9000/someUrl'
  });
}]);
````

Angular vous retourne ce qu'on appelle une promise ce qui est une façon très efficace et lisible de gérer les callbacks. 

Les promises sont rattachées à la fonction qui les as créées via la notation `.myPromise()`. 

Tout naturellement, nous avons la main sur `success` et `error` :
````
myApp.controller('MainCtrl', ['$scope', function ($scope) {
  $http({
    method: 'GET',
    url: '//localhost:9000/someUrl'
  })
  .success(function (data, status, headers, config) {
    // données récupérées avec succès
  })
  .error(function (data, status, headers, config) {
    // erreur de récupération :(
  });
}]);
````

Tout à fait lisible.

C'est ici que nous fusionnons la vue et le serveur en y attachant un modèle ou en mettant le modèle à jour dans le DOM. Disons que tout est déjà mis en place et affichons un nom d'utilisateur dans le DOM à partir d'un appel Ajax.

En théorie, nous devrions concevoir notre JSON en premier, ce qui affecte la façon dont on l'attache à nos données. 

Faisons simple, voici ce que le serveur nous fournit :
````
{
  "user": {
    "name": "Todd Motto",
    "id": "80138731"
  }
}
````

Nous allons recevoir un Object (que nous appellerons data, on peut voir que data est passé à notre handler) et devons interagir avec la propriété `data.user`. Dans `data.user`, nous trouvons `name` et `id`. Obtenir leur valeur est assez simple, il nous suffit par exemple de faire appel à `data.user.name` ce qui nous donne 'Todd Motto’.

Le code JavaScript :

````
myApp.controller('UserCtrl', ['$scope', '$http', function ($scope, $http) {

  // Crée un Object user
  $scope.user = {};

  // Initialise le modèle avec une chaîne vide
  $scope.user.username = '';

  // Nous voulons effectuer la requête
  // et obtenir le nom de l'utilisateur
  $http({
    method: 'GET',
    url: '//localhost:9000/someUrlForGettingUsername'
  })
  .success(function (data, status, headers, config) {
    // Ici nous assignons cet utilisateur à
    // notre modèle existant !
    $scope.user.username = data.user.name;
  })
  .error(function (data, status, headers, config) {
    // Une erreur est survenue
  });
}]);
````

Il nous suffit maintenant de faire ceci dans le DOM :
````
<div ng-controller="UserCtrl">
  <p>{{ user.username }}</p>
</div>
````

Ceci va afficher le nom de l'utilisateur. 

Nous allons faire un pas de plus et comprendre le data-binding déclaratif et c'est là que ça devient vraiment intéressant.

##Data-binding déclaratif

La philosophie d'Angular est de créer du HTML dynamique, riche en fonctionnalités et d'effectuer, de façon transparente, beaucoup de choses dont on n'oserait à peine rêver côté web client. 

Et c'est exactement ce qu'ils ont fait.

Imaginons que nous venons de faire une requête Ajax pour récupérer une liste d'emails avec leur sujet ainsi que leur date d'envoi et souhaitons les afficher dans le DOM. 

C'est là qu'Angular montre toute sa force. Nous allons tout d'abord devoir écrire un contrôleur d'emails :

````
myApp.controller('EmailsCtrl', ['$scope', function ($scope) {

  // Crée un Object emails
  $scope.emails = {};

  // Nous écrivons ici en dur les données normalement
  // reçues du serveur
  $scope.emails.messages = [{
        "from": "Steve Jobs",
        "subject": "I think I'm holding my phone wrong :/",
        "sent": "2013-10-01T08:05:59Z"
    },{
        "from": "Ellie Goulding",
        "subject": "I've got Starry Eyes, lulz",
        "sent": "2013-09-21T19:45:00Z"
    },{
        "from": "Michael Stipe",
        "subject": "Everybody hurts, sometimes.",
        "sent": "2013-09-12T11:38:30Z"
    },{
        "from": "Jeremy Clarkson",
        "subject": "Think I've found the best car... In the world",
        "sent": "2013-09-03T13:15:11Z"
    }];

}]);
````

Nous devons maintenant faire le lien avec notre HTML. 

C'est là que nous allons utiliser le binding déclaratif pour indiquer ce que va faire l'application - créer notre premier morceau de HTML dynamique. 

Nous allons utiliser la directive ng-repeat d'Angular qui va parcourir les données et générer un résultat sans que nous ayons à faire de callbacks ou de changements d'état, c'est gratuit :

````
<ul>
  <li ng-repeat="message in emails.messages">
    <p>From: {{ message.from }}</p>
    <p>Subject: {{ message.subject }}</p>
    <p>{{ message.sent | date:'MMM d, y h:mm:ss a' }}</p>
  </li>
</ul>
````
J'ai également ajouté un filtre pour vous montrer comment afficher correctement une date UTC.

Documentez-vous sur les directives `ng-*` d'Angular, cela vous permettra d'exploiter pleinement le binding de directives, cela vous montre comment faire le lien entre les données du serveur, la couche modèle et la vue et comment afficher les données.

##Les fonctions de scope

Après le binding de directives, les fonctions de scope sont la suite logique dans la création d'une application. 

Voici une fonction assez simple nous permettant de supprimer un email dans notre collection :

````
myApp.controller('MainCtrl', ['$scope', function ($scope) {

  $scope.deleteEmail = function (index) {
    $scope.emails.messages.splice(index, 1)
  };

}]);
````

Astuce: Il est important de penser en termes de suppression au niveau du modèle. Vous ne touchez à rien de ce qui concerne le DOM, Angular est un framework MVC et se charge de tout grâce à son binding de données à deux sens et à son fonctionnement sans callbacks, il vous suffit d'organiser votre code de façon à ce qu'il réponde à vos données !

L'attachement des fonctions au scope se fait également par le biais des directives `ng-*`, dans le cas suivant, il s'agit de `ng-click` :
````
<a ng-click="deleteEmail($index)">Delete email</a>
````

Ceci est très différent d'un événement click inline et ce pour plusieurs raisons dont je vous parlerai bientôt. Comme vous pouvez le voir, je passe `$index` en paramètre. Angular sait quel élément vous êtes en train de supprimer :

Résulat (supprimez quelques emails !) :

##Méthodes de DOM déclaratives

Nous allons maintenant voir les méthodes de DOM. Elles sont comme les directives et permettent d'ajouter au DOM des fonctionnalités pour lesquelles vous auriez, en temps normal, beaucoup de code à écrire. 

Un très bon exemple serait une navigation déroulante. En utilisant `ng-show` et `ng-click` nous pouvons mettre cela en place très simplement :

````
<a href="" ng-click="toggle = !toggle">Afficher menu</a>
<ul ng-show="toggle">
  <li>Lien 1</li>
  <li>Lien 2</li>
  <li>Lien 3</li>
</ul>
````
C'est ici que MVVM entre en jeu. Comme vous pouvez le voir, il n'y a pas de contrôleur à écrire. Nous reviendrons bientôt sur MVVM.

##Les expressions

C'est une des choses que je préfère dans Angular, ce pour quoi on utilise habituellement JavaScript en écrivant beaucoup de code répétitif.

Avez-vous déjà fait ceci ?

````
elem.onclick = function (data) {
  if (data.length === 0) {
    otherElem.innerHTML = 'No data';
  } else {
    otherElem.innerHTML = 'My data';
  }
};
````

Ce pourrait par exemple être le callback d'une requête GET dans lequel vous modifiez le DOM en fonction de l'état des données. Angular vous permet de faire la même chose sans écrire une ligne de JavaScript !

````
<p>{{ data.length > 0 && 'My data' || 'No data' }}</p>
````

Ce morceau de DOM va être automatiquement mis à jour, sans besoin de callback, au fur et à mesure que votre application récupère les données. 

Il vous indiquera si des données sont présentes ou non. 

Il y a une de très nombreux cas d'usage et Angular le gère automatiquement grâce au binding de données à deux sens.

##Routage et vues dynamiques

La philosophie des applications côté client (et des applications web en général) est assez simple : Vous avez un header, un footer, une sidebar et le contenu au milieu injecte du contenu par magie en fonction de l'URL.

Angular facilite beaucoup la mise en place d'un tel mécanisme, ce qu'on appellerait les vues dynamiques. Les vues sont injectées dynamiquement au travers de $routeProvider, en fonction de l'URL.

````
myApp.config(['$routeProvider', function ($routeProvider) {

  /**
   * $routeProvider
   */
  $routeProvider
  .when('/', {
    templateUrl: 'views/main.html'
  })
  .otherwise({
    redirectTo: '/'
  });

}]);
````

Lorsque vous êtes sur l'URL `/` (la page d'accueil du site), le template utilisé est `main.html`. Vous devez appeler votre vue initiale `main.html` puisque vous avez déjà un fichier `index.html` contenant votre site en page unique. Il est très simple d'ajouter des vues supplémentaires en fonction de l'URL :

````
myApp.config(['$routeProvider', function ($routeProvider) {

  /**
   * $routeProvider
   */
  $routeProvider
  .when('/', {
    templateUrl: 'views/main.html'
  })
  .when('/emails', {
    templateUrl: 'views/emails.html'
  })
  .otherwise({
    redirectTo: '/'
  });

}]);
````

Nous pouvons par exemple charger le HTML de notre liste d'emails quand on visite l'URL `emails.html.` Vous pouvez créer une application sophistiquée avec assez peu d'efforts.

`$routeProvider` propose d'autres fonctionnalités qui valent la peine de fouiller un peu, mais avec ceci vous avez déjà de quoi faire des choses intéressantes. Il y a, par exemple, les intercepteurs `$http` qui émettent des événements lorsqu'une requête Ajax est en cours, ce qui permettrait d'afficher un spinner pendant que les données sont récupérées.

##Données statiques globales

Gmail récupère une bonne partie de ses données en les écrivant en JSON dans la page (inspectez les sources de la page). Si vous voulez accéder instantanément à vos données dès le chargement de l'application et accélérer son lancement, cette technique est très pratique.

Lorsque je développe nos applications, des balises issues de Java sont insérées dans le DOM et, une fois l'application chargée, les données viennent du serveur. Je parle ici de Java mais vous pouvez utiliser n'importe quel langage côté serveur. Voici comment ajouter le JSON dans votre page et comment le charger ensuite depuis un contrôleur :

````
<!-- dans index.html (en bas de page) -->
<script>
window.globalData = {};
globalData.emails = <baliseJavaPourGenererLesMessages>;
</script>
````

Ma balise Java va être remplacée par les bonnes données au moment de l'affichage et Angular va affiche instantanément les emails. Récupérez simplement les données dans un contrôleur :

````
myApp.controller('EmailsCtrl', ['$scope', function ($scope) {

    $scope.emails = {};

    // Assigner les données initiales !
    $scope.emails.messages = globalData.emails;

}]);
````

##Minification (réduction du code)

Je vais parler rapidement de minification avec le code Angular. Vous avez probablement joué un peu avec Angular et avez peut-être utilisé un minifier… et eu une erreur !

Minifier le code AngularJS est simple, vous devez simplement spécifier les injections de dépendances dans un tableau, avant la fonction :

````
myApp.controller('MainCtrl',
['$scope', 'Dependency', 'Service', 'Factory',
function ($scope, Dependency, Service, Factory) {

  // code

}]);
````

Une fois minifié :

````
myApp.controller('MainCtrl',
['$scope', 'Dependency', 'Service', 'Factory',
function (a,b,c,d) {

  // a = $scope
  // b = Dependency
  // c = Service
  // d = Factory

  // $scope alias usage
  a.someFunction = function () {...};

}]);
````

Rappelez-vous bien de garder la liste des injections dans le même ordre que les paramètres de la fonction, vous risqueriez un bon mal de crâne dans le cas contraire.

#Différences avec MVC et MVVM

Nous arrivons à la fin de ce long article sur AngularJS. Je vais cependant faire une passe rapide sur les différences avec MVC/MVVM dont AngularJS est fier :

* MVC: parle avec un contrôleur, Modèle-Vue-Contrôleur
* MVVM: encapsule une binding de données déclaratif qui, techniquement, se parle à lui-même. Modèle-Vue-Vue-Modèle. 
Le modèle parle à la vue et la vue peut parler au modèle. Le binding de données à deux sens d'Angular permet de garder cette synchronisation sans rien avoir à écrire. Cela vous permet d'écrire de la logique sans contrôleur.
Un exemple rapide, vous pouvez utiliser `ng-repeat` sans contrôleur en fournissant directement les données :

````
<li ng-repeat="number in [1,2,3,4,5,6,7,8,9]">
  {{ number }}
</li>
````

Pour un test rapide c'est acceptable mais je vous conseille de toujours avoir un contrôleur lorsque vous développez.

# HTML5 Web Components

Comme nous l'avons vu plus haut, AngularJS permet de créer des éléments personnalisés :

````
<myCustomElement></myCustomElement>
````

Cela permet d'aligner le web sur le futur de HTML5 qui introduit les web components et l'élément `<template>`. 

Angular nous permet de les utiliser dès aujourd'hui. Les web components mélangent éléments personnalisés et injection dynamique de JavaScript pour peupler dynamiquement la vue, c'est très excitant et déjà possible avec Angular !

Ils ont un cran d'avance et permettent de s'assurer que ce qui arrive fonctionne - chapeau bas.

# Commentaires de scope

Les commentaires de scopes sont, selon moi, utiles dans mon workflow. Plutôt que d'ajouter des commentaires comme ceci dans mon HTML :

````
<!-- header -->
<header>
  Stuff.
</header>
<!-- /header -->
````

Lorsque l'on parle d'Angular, il faut penser en termes de vues et de scopes, pas de DOM ! 

Les scopes sont cloisonnés, ce qui signifie que, à moins de partager délibérément les données entre les contrôleurs, vos données sont encapsulées et inaccessibles de l'extérieur. Je trouve que noter les régions cloisonnées aide beaucoup :

Les commentaires de scopes sont, selon moi, utiles dans mon workflow. Plutôt que d'ajouter des commentaires comme ceci dans mon HTML :

````
<!-- scope: MainCtrl -->
<div class="content" ng-controller="MainCtrl">

</div>
<!-- /scope: MainCtrl -->
````

# Debugger AngularJS

Il y a une extension Chrome géniale que les gens de Google recommandent pour développer et debugger avec Angular, il s'agit de Batarang.

À lire également

Apprenez comment créer votre propre directive depuis un script ou un plugin.
