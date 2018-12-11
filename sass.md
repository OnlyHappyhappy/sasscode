#  1.变量的使用
## 1-1. 变量声明
###安装此插件
Live Sass Compiler

sass变量声明使用的是$加变量名
    $nav-color: #F90;
    nav {
    $width: 100px;//这里的$width在里面声明的可以直接使用,但是外面的就是用不了
    width: $width;
    color: $nav-color;
    }

    //编译后

    nav {
    width: 100px;
    color: #F90;
    }
## 1-2. 变量引用
在声明变量时，变量值也可以引用其他变量。
    $highlight-color: #F90;
    $highlight-border: 1px solid $highlight-color;
    .selected {
    border: $highlight-border;
    }

    //编译后

    .selected {
    border: 1px solid #F90;
    }

## 1-3. 变量名用中划线还是下划线分隔
sass的变量名可以与css中的属性名和选择器名称相同，包括中划线和下划线。用中划线声明的变量可以使用下划线的方式引用，反之亦然。
    $link-color: blue;
    a {
    color: $link_color;
    }

    //编译后

    a {
    color: blue;
    } 
    //但是在sass中纯css部分不互通，比如类名、ID或属性名。




#2. 嵌套CSS 规则

sass在输出css时会帮你把这些嵌套规则处理好，避免你的重复书写。
    #content {
    article {
        h1 { color: #333 }
        p { margin-bottom: 1.4em }
    }
    aside { background-color: #EEE }
    }
    
     /* 编译后 */
    content article h1 { color: #333 }
    content article p { margin-bottom: 1.4em }
    content aside { background-color: #EEE }
## 2-1. 父选择器的标识符&;
特殊的sass选择器，即父选择器。在使用嵌套规则时，父选择器能对于嵌套规则如何解开提供更好的控制。
    article a {
    color: blue;
    &:hover { color: red }
    }

## 2-2. 群组选择器的嵌套;
在CSS里边，选择器h1h2和h3会同时命中h1元素、h2元素和h3元素。与此类似，.button button会命中button元素和类名为.button的元素。这种选择器称为群组选择器。群组选择器 的规则会对命中群组中任何一个选择器的元素生效。也就是平时的并集
    .button, button {
    margin: 0;
    }
##2-3. 子组合选择器和同层组合选择器：>、+和~;
上边这三个组合选择器必须和其他选择器配合使用，以指定浏览器仅选择某种特定上下文中的元素。
    article section { margin: 5px }
    article > section { border: 1px solid #ccc }
    header + p { font-size: 1.1em }
    article ~ article { border-top: 1px dashed #ccc }

##2-4. 嵌套属性;
在sass中，除了CSS选择器，属性也可以进行嵌套。尽管编写属性涉及的重复不像编写选择器那么糟糕，但是要反复写border-styleborder-widthborder-color以及border-*等也是非常烦人的。在sass中，你只需敲写一遍border：
    nav {
    border: {
    style: solid;
    width: 1px;
    color: #ccc;
    }
    }

    <!-- 编译后 -->
    nav {
    border-style: solid;
    border-width: 1px;
    border-color: #ccc;
    }
    嵌套属性的规则是这样的：把属性名从中划线-的地方断开，在根属性后边添加一个冒号:，紧跟一个{ }块，把子属性部分写在这个{ }块中。就像css选择器嵌套一样，sass会把你的子属性一一解开，把根属性和子属性部分通过中划线-连接起来，最后生成的效果与你手动一遍遍写的css样式一样.
    
    //对于属性的缩写形式，你甚至可以像下边这样来嵌套，指明例外规则：
    nav {
    border: 1px solid #ccc {
    left: 0px;
    right: 0px;
    }
    }

    这比下边这种同等样式的写法要好：
    nav {
    border: 1px solid #ccc;
    border-left: 0px;
    border-right: 0px;
    }

#3. 导入SASS文件;
css有一个特别不常用的特性，即@import规则，它允许在一个css文件中导入其他css文件。然而，后果是只有执行到@import时，浏览器才会去下载其他css文件，这导致页面加载起来特别慢。

sass也有一个@import规则，但不同的是，sass的@import规则在生成css文件时就把相关文件导入进来。这意味着所有相关的样式被归纳到了同一个css文件中，而无需发起额外的下载请求。另外，所有在被导入文件中定义的变量和混合器（参见2.5节）均可在导入文件中使用。

使用sass的@import规则并不需要指明被导入文件的全名。你可以省略.sass或.scss文件后缀（见下图）。这样，在不修改样式表的前提下，你完全可以随意修改你或别人写的被导入的sass样式文件语法，在sass和scss语法之间随意切换。举例来说，@import"sidebar";这条命令将把sidebar.scss文件中所有样式添加到当前样式表中。

  其实就是会和我们less导入文件一样

## 3-1. 使用SASS部分文件;
当通过@import把sass样式分散到多个文件时，你通常只想生成少数几个css文件。那些专门为@import命令而编写的sass文件，并不需要生成对应的独立css文件，这样的sass文件称为局部文件。对此，sass有一个特殊的约定来命名这些文件。

此约定即，sass局部文件的文件名以下划线开头。这样，sass就不会在编译时单独编译这个文件输出css，而只把这个文件用作导入。当你@import一个局部文件时，还可以不写文件的全名，即省略文件名开头的下划线。举例来说，你想导入themes/_night-sky.scss这个局部文件里的变量，你只需在样式表中写@import "themes/night-sky";。

##3-2. 默认变量值;
有时有些变量的sass文件以及sass混入器的一些样式需要在多个页面甚至多个项目中使用时，且不是他的所有样式都符合需求,这时候可以使用默认变量值更改
    $fancybox-width: 400px !default;
    .fancybox {
    width: $fancybox-width;
    }
##3-3. 嵌套导入;
跟原生的css不同，sass允许@import命令写在css规则内。这种导入方式下，生成对应的css文件时，局部文件会被直接插入到css规则内导入它的地方。
   举例说明，有一个名为_blue-theme.scss的局部文件，内容如下
    aside {
    background: blue;
    color: white;
    }

    然后把它导入到一个CSS规则内，如下所示：
    .blue-theme {@import "blue-theme"}

    生成的结果跟你直接在.blue-theme选择器内写_blue-theme.scss文件的内容完全一样。

    .blue-theme {
    aside {
        background: blue;
        color: #fff;
    }
    }

    被导入的局部文件中定义的所有变量和混合器，也会在这个规则范围内生效。这些变量和混合器不会全局有效，这样我们就可以通过嵌套导入只对站点中某一特定区域运用某种颜色主题或其他通过变量配置的样式。

## 3-4. 原生的CSS导入;

由于sass兼容原生的css，所以它也支持原生的CSS@import。尽管通常在sass中使用@import时，sass会尝试找到对应的sass文件并导入进来，但在下列三种情况下会生成原生的CSS@import，尽管这会造成浏览器解析css时的额外下载：

    被导入文件的名字以.css结尾；
    被导入文件的名字是一个URL地址（比如http://www.sass.hk/css/css.css），由此可用谷歌字体API提供的相应服务；
    被导入文件的名字是CSS的url()值。
    这就是说，你不能用sass的@import直接导入一个原始的css文件，因为sass会认为你想用css原生的@import。但是，因为sass的语法完全兼容css，所以你可以把原始的css文件改名为.scss后缀，即可直接导入了。

#4. 静默注释
即静默注释，其内容不会出现在生成的css文件中。静默注释的语法跟JavaScriptJava等类C的语言中单行注释的语法相同，它们以//开头，注释内容直到行末。
    body {
    color: #333; // 这种注释内容不会出现在生成的css文件中
    padding: 0; /* 这种注释内容会出现在生成的css文件中 */
    }
     上面的//的注释是不会在解析出来的css里面显示的

#5. 混合器;
需要大段大段的重用样式的代码，独立的变量就没办法应付这种情况了。你可以通过sass的混合器实现大段样式的重用。混合器使用@mixin标识符定义。 
    @mixin rounded-corners {
    -moz-border-radius: 5px;
    -webkit-border-radius: 5px;
    border-radius: 5px;
    }

    //调用直接是@include来使用这个混合器
    notice {
    background-color: green;
    border: 2px solid #00aa00;
    @include rounded-corners; //这里就是使用了上面定义的混合器
    }

    最终生成的结果是:
     .notice {
    background-color: green;
    border: 2px solid #00aa00;
    -moz-border-radius: 5px;
    -webkit-border-radius: 5px;
    border-radius: 5px;
    }
            
    //这个是less ,长得像函数一样,调用的时候直接.position-center()
   
     .position-center(){
        position: absolute;
        left: 50%;
        transform: translateX(-50%);
    }

## 5-1. 何时使用混合器
利用混合器，可以很容易地在样式表的不同地方共享样式。如果你发现自己在不停地重复一段样式，那就应该把这段样式构造成优良的混合器，尤其是这段样式本身就是一个逻辑单元，比如说是一组放在一起有意义的属性。

判断一组属性是否应该组合成一个混合器，一条经验法则就是你能否为这个混合器想出一个好的名字。如果你能找到一个很好的短名字来描述这些属性修饰的样式，比如rounded-cornersfancy-font或者no-bullets，那么往往能够构造一个合适的混合器。如果你找不到，这时候构造一个混合器可能并不合适。

混合器在某些方面跟css类很像。都是让你给一大段样式命名，所以在选择使用哪个的时候可能会产生疑惑。最重要的区别就是类名是在html文件中应用的，而混合器是在样式表中应用的。这就意味着类名具有语义化含义，而不仅仅是一种展示性的描述：用来描述html元素的含义而不是html元素的外观。而另一方面，混合器是展示性的描述，用来描述一条css规则应用之后会产生怎样的效果。

在之前的例子中，.notice是一个有语义的类名。如果一个html元素有一个notice的类名，就表明了这个html元素的用途：向用户展示提醒信息。rounded-corners混合器是展示性的，它描述了包含它的css规则最终的视觉样式，尤其是边框角的视觉样式。混合器和类配合使用写出整洁的html和css，因为使用语义化的类名亦可以帮你避免重复使用混合器。为了保持你的html和css的易读性和可维护性，在写样式的过程中一定要铭记二者的区别。

有时候仅仅把属性放在混合器中还远远不够，可喜的是，sass同样允许你把css规则放在混合器中。

    总结就是:重复利用代码段的地方,且需要起一个容易被记住的名字,以免建立了混合器,虽然是需要多次使用,可是实际开发却没有真的使用到他,所以会导致网页的额外加载
## 5-2. 混合器中的CSS规则
    
sass混合器中不仅可以包含属性，也可以包含css规则，包含选择器和选择器中的属性，如下代码:
    @mixin no-bullets {
    list-style: none;
    li {
        list-style-image: none;
        list-style-type: none;
        margin-left: 0px;
    }
    }
当一个包含css规则的混合器通过@include包含在一个父规则中时，在混合器中的规则最终会生成父规则中的嵌套规则。举个例子，看看下边的sass代码，这个例子中使用了no-bullets这个混合器：
    ul.plain {
    color: #444;
    @include no-bullets;
    }

sass的@include指令会将引入混合器的那行代码替换成混合器里边的内容。最终，上边的例子如下代码:
    ul.plain {
    color: #444;
    list-style: none;
    }
    ul.plain li {
    list-style-image: none;
    list-style-type: none;
    margin-left: 0px;
    }

    混合器中的规则甚至可以使用sass的父选择器标识符&。使用起来跟不用混合器时一样，sass解开嵌套规则时，用父规则中的选择器替代&。

    如果一个混合器只包含css规则，不包含属性，那么这个混合器就可以在文档的顶部调用，写在所有的css规则之外。如果你只是为自己写一些混合器，这并没有什么大的用途，但是当你使用一个类似于Compass的库时，你会发现，这是提供样式的好方法，原因在于你可以选择是否使用这些样式。

##5-3. 给混合器传参
混合器并不一定总得生成相同的样式。可以通过在@include混合器时给混合器传参，来定制混合器生成的精确样式。当@include混合器时，参数其实就是可以赋值给css属性值的变量。如果你写过JavaScript，这种方式跟JavaScript的function很像：

    @mixin link-colors($normal, $hover, $visited) {
    color: $normal;
    &:hover { color: $hover; }
    &:visited { color: $visited; }
    }
当混合器被@include时，你可以把它当作一个css函数来传参。如果你像下边这样写：
    a {
    @include link-colors(blue, red, green);
    }

    //Sass最终生成的是：

    a { color: blue; }
    a:hover { color: red; }
    a:visited { color: green; }
当你@include混合器时，有时候可能会很难区分每个参数是什么意思，参数之间是一个什么样的顺序。为了解决这个问题，sass允许通过语法$name: value的形式指定每个参数的值。这种形式的传参，参数顺序就不必再在乎了，只需要保证没有漏掉参数即可：
    a {
    @include link-colors(
        $normal: blue,
        $visited: green,
        $hover: red
    );
    }
#6. 使用选择器继承来精简CSS
使用sass的时候，最后一个减少重复的主要特性就是选择器继承。基于Nicole Sullivan面向对象的css的理念，选择器继承是说一个选择器可以继承为另一个选择器定义的所有样式。这个通过@extend语法实现，如下代码: 
    //通过选择器继承继承样式
    .error {
    border: 1px solid red;
    background-color: #fdd;
    }
    .seriousError {
    @extend .error;
    border-width: 3px;
    }
    
    //总结:如果需要共享另外一个类的一些样式的话,可以直接使用@extend 加上可以使用这个类名
    

