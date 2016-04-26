<!--more-->

### 1\.内置随机数生成器

    Math.random(); //产生一个0到1之间的浮点数。
    Math.floor(Math.random()*10+1); //1-10
    Math.floor(Math.random()*24);//0-23


### 2\.基于时间的随机生成器

    var now=new Date();
    var number = now.getSeconds(); //产生一个基于目前时间的0到59的整数。

    var now=new Date();
    var number = now.getSeconds()%43; //产生一个基于目前时间的0到42的整数。


### 3\.一个优秀的Javascript随机数生成器

    <script language="JavaScript">
    <!--
    // The Central Randomizer 1.3 (C) 1997 by Paul Houle (houle@msc.cornell.edu)
    // See: http://www.msc.cornell.edu/~houle/javascript/randomizer.html
    rnd.today=new Date();
    rnd.seed=rnd.today.getTime();
    function rnd() {
    　　　　rnd.seed = (rnd.seed*9301+49297) % 233280;
    　　　　return rnd.seed/(233280.0);
    };
    function rand(number) {
    　　　　return Math.ceil(rnd()*number);
    };
    // end central randomizer. -->
    </script>


### 4\.给定某个范围生成多个随机整数

简明的实现,效率低

    ```Javascript
    function(min,max,num){
        var arr = [];
        while(arr.length < num){
          var randomnumber=Math.ceil(Math.random()*(max - min)) + min;
          var found=false;
          for(var i=0;i<arr.length;i++){
            if(arr[i]==randomnumber){found=true;break;}
          }
          if(!found) arr[arr.length]=randomnumber;
        }
    }
    ```


Fisher-Yates shuffle洗牌算法

    ```Javascript
    // removes n random elements from array this
    // and returns them
    // splice(index,n,value) 删除从index开始的n个元素，并替换为value值
    Array.prototype.pick = function(n) {
        if(!n || !this.length) return [];
        var i = Math.floor(this.length*Math.random());
        return this.splice(i,1).concat(this.pick(n-1));
    }

    // returns n unique random numbers between min and max
    function pick(n, min, max) {
        var a = [], i = max;
        while(i >= min) a.push(i--);
        return a.pick(n);
    }

    pick(8,1,100);
    ```
