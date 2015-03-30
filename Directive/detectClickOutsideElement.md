````
module.directive('clickAnywhereButHere', function ($document) {
    return {
        restrict: 'A',
        link: function (scope, elem, attr, ctrl) {
            elem.bind('click', function (e) {
                // this part keeps it from firing the click on the document.
                e.stopPropagation();
            });
            $document.bind('click', function () {
                // magic here.
                scope.$apply(attr.clickAnywhereButHere);
            })
        }
    }
});

<div class="dropDownSelected" click-anywhere-but-here="$context.gateways.isOpened=false" ng-click="$context.gateways.isOpened = true">

<div class="dropDownGatewayOptions" style="display:block" ng-show="$context.gateways.isOpened">test</div>
````
