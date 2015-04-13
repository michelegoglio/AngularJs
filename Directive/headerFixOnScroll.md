````
app = angular.module('myApp', []);
app.directive("scroll", function ($window) {
    return function(scope, element, attrs) {
        angular.element($window).bind("scroll", function() {
            scrollTop = this.pageYOffset;
            header = document.getElementById("offsetHeader");
            headerHeight = header.offsetHeight;
            offsetHeader = header.offsetTop + headerHeight;
            offsetTodayList = document.getElementById("offsetTodayList").offsetTop;
            headerToFix =  document.getElementById("headerToFix"); 
            widthHeaderToFix = headerToFix.offsetWidth;   
             if (hasClass(headerToFix, 'fixed') == false) {
                if(scrollTop >= offsetHeader && scrollTop <= offsetTodayList){
                    headerToFix.style.display = 'none';
                    headerToFix.style.width = widthHeaderToFix + "px";
                    headerToFix.classList.add("fixed");
                    headerToFix.style.display = 'block';
                }
             } else {
                if(scrollTop > offsetTodayList ){
                    headerToFix.style.display = 'none';
                } else{
                    headerToFix.style.display = 'block';
                }
                if(scrollTop < offsetHeader){
                    headerToFix.classList.remove("fixed");
                }

             }
            scope.$apply();
        });

        function hasClass(element, cls) {
          return (' ' + element.className + ' ').indexOf(' ' + cls + ' ') > -1;
        }
    };
});
````

````
  <section class="SDVResults">
    <h1 id="offsetHeader">Offset header</h1>
    <div id="headerToFix">
           header to fix           
    </div>
    <div>
        content
    </div>
    <div id="offsetTodayList">
        content offset today list
    </div>      
  </section>
    
</div>
````
