#ng_route_ext
extension ng_route pour animations des vues

[ng_route_ext](https://github.com/allaud/ng-route-ext)

[Demo](http://allaud.github.io/ng-route-ext/example/)

**HTML**
````
<html ng-app="Test">
<head>
  <script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/angularjs/1.3.4/angular.min.js"></script>
  <script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/angularjs/1.3.0/angular-animate.min.js"></script>
  <script type="text/javascript" src="ng_route_ext.js"></script>
</head>
<body>
    <a href="/ng-route-ext/example/">index</a> | 
    <a href="/ng-route-ext/example/pictures">pictures</a> | 
    <a href="/ng-route-ext/example/games">games</a>
    
    <ng-view class="frame">
      <div class="index">INDEX</div>
    </ng-view>
</body>
</html>
````

**Styles**
````
<style type="text/css">
  .index{background: grey;width: 100%; height: 100%;position: absolute;}
  .pictures{background: red;width: 100%; height: 100%;position: absolute;}
  .frame {
    position: absolute;
    top: 40px;
    left: 0;
    height: 100%;
    width: 100%;
    z-index: 3;
  }
  .from-bottom.ng-enter, .from-bottom.ng-leave,
  .from-bottom.ng-enter-active, .from-bottom.ng-leave-active{
      transition: all .5s ease;
      z-index: 1;
  }
  .from-bottom.ng-enter {
  }
  .from-bottom.ng-enter-active {
  }
  .from-bottom.ng-leave {
    transform: translateY(0);
  }
  .from-bottom.ng-leave-active {
    transform: translateY(-100%);
  }
  .to-top.ng-enter, .to-top.ng-leave,
  .to-top.ng-enter-active, .to-top.ng-leave-active{
      transition: all .5s ease;
      z-index: 1;
  }
  .to-top.ng-enter {
    transform: translateY(100%);
  }
  .to-top.ng-enter-active {
    transform: translateY(0);
  }
  .to-top.ng-leave {
  }
  .to-top.ng-leave-active {
  }
  .from-top.ng-enter, .from-top.ng-leave,
  .from-top.ng-enter-active, .from-top.ng-leave-active{
      transition: all .5s ease;
      z-index: 1;
  }
  .from-top.ng-enter {
    transform: translateY(-100%);
  }
  .from-top.ng-enter-active {
    transform: translateY(0);
  }
  .from-top.ng-leave {
  }
  .from-top.ng-leave-active {
  }
  .to-bottom.ng-enter, .to-bottom.ng-leave,
  .to-bottom.ng-enter-active, .to-bottom.ng-leave-active{
      transition: all .5s ease;
      z-index: 1;
  }
  .to-bottom.ng-enter {
    transform: translateY(0);
  }
  .to-bottom.ng-enter-active {
    transform: translateY(100%);
  }
  .to-bottom.ng-leave {
  }
  .to-bottom.ng-leave-active {
  }
  .to-left.ng-enter, .to-left.ng-leave,
  .to-left.ng-enter-active, .to-left.ng-leave-active{
      transition: all .5s ease;
      z-index: 1;
  }
  .to-left.ng-enter {
  }
  .to-left.ng-enter-active {
  }
  .to-left.ng-leave {
    transform: translateX(0);
  }
  .to-left.ng-leave-active {
    transform: translateX(-100%);
  }
  .to-right.ng-enter, .to-right.ng-leave,
  .to-right.ng-enter-active, .to-right.ng-leave-active{
      transition: all .5s ease;
      z-index: 1;
  }
  .to-right.ng-enter {
  }
  .to-right.ng-enter-active {
  }
  .to-right.ng-leave {
    transform: translateX(0);
  }
  .to-right.ng-leave-active {
    transform: translateX(100%);
  }
  .from-right.ng-enter, .from-right.ng-leave,
  .from-right.ng-enter-active, .from-right.ng-leave-active{
      transition: all .5s ease;
      z-index: 1;
  }
  .from-right.ng-enter {
    transform: translateX(100%);
  }
  .from-right.ng-enter-active {
    transform: translateX(0);
  }
  .from-right.ng-leave {
  }
  .from-right.ng-leave-active {
  }
  .from-left.ng-enter, .from-left.ng-leave,
  .from-left.ng-enter-active, .from-left.ng-leave-active{
      transition: all .5s ease;
      z-index: 1;
  }
  .from-left.ng-enter {
    transform: translateX(-100%);
  }
  .from-left.ng-enter-active {
    transform: translateX(0);
  }
  .from-left.ng-leave {
  }
  .from-left.ng-leave-active {
  }
  </style>
  ````
  
**Script**
````
angular.module('Test', ['ngRoute', 'ngAnimate']).config(function(){});
angular.module('Test').config(function($routeProvider, $locationProvider){
  $routeProvider
    .when('/', {
      templateUrl: 'index.html',
      controller: 'TestController',
      name: 'index',
      animations: {
        pictures: ['to-bottom', 'from-top'],
        games: ['to-right', 'from-left']
      }
    })
    .when('/pictures', {
      templateUrl: 'pictures.html',
      controller: 'TestController',
      name: 'pictures',
      animations: {
        index: ['from-bottom', 'to-top'],
        games: ['to-right', 'from-left']
      }
    })
    .when('/games', {
      templateUrl: 'games.html',
      controller: 'TestController',
      name: 'games',
      animations: {
        index: ['to-left', 'from-right'],
        pictures: ['to-left', 'from-right']
      }
    });
    $locationProvider.html5Mode(true);
}).run(function($templateCache){
    $templateCache.put("index.html","<div class='index'>INDEX</div>");
    $templateCache.put("pictures.html","<div class='pictures'>PICTURES</div>");
    $templateCache.put("games.html","<div class='games'>GAMES</div>");
}).controller('TestController', function($scope){
});
````
