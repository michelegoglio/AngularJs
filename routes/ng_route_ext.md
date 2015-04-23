#ng_route_ext
extension ng_route pour animations des vues

[ng_route_ext](https://github.com/allaud/ng-route-ext)

[Demo](http://allaud.github.io/ng-route-ext/example/)

````
<head>
  <script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/angularjs/1.3.4/angular.min.js"></script>
  <script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/angularjs/1.3.0/angular-animate.min.js"></script>
  <script type="text/javascript" src="ng_route_ext.js"></script>
</head>
<body>
    <a href="/ng-route-ext/example/">index</a> | 
    <a href="/ng-route-ext/example/pictures">pictures</a> | 
    <a href="/ng-route-ext/example/games">games</a>
    
    <ng-view class="frame ng-scope">
      <div class="index ng-scope">INDEX</div>
    </ng-view>
</body>
````
