
jquery选择器

jquery用法思想一
选择某个网页元素，然后对它进行某种操作

jquery选择器
jquery选择器可以快速地选择元素，选择规则和css样式相同，使用length属性判断是否选择成功。

```
$(document) //选择整个文档对象
$('li') //选择所有的li元素
$('#myId') //选择id为myId的网页元素
$('.myClass') // 选择class为myClass的元素
$('input[name=first]') // 选择name属性等于first的input元素
$('#ul1 li span') //选择id为为ul1元素下的所有li下的span元素

```

对选择集进行修饰过滤(类似CSS伪类)

```
$('#ul1 li:first') //选择id为ul1元素下的第一个li
$('#ul1 li:odd') //选择id为ul1元素下的li的奇数行
$('#ul1 li:eq(2)') //选择id为ul1元素下的第3个li
$('#ul1 li:gt(2)') // 选择id为ul1元素下的前三个之后的li
$('#myForm :input') // 选择表单中的input元素
$('div:visible') //选择可见的div元素

```

对选择集进行函数过滤

```
$('div').has('p'); // 选择包含p元素的div元素
$('div').not('.myClass'); //选择class不等于myClass的div元素
$('div').filter('.myClass'); //选择class等于myClass的div元素
$('div').first(); //选择第1个div元素
$('div').eq(5); //选择第6个div元素

```

选择集转移

```
$('div').prev('p'); //选择div元素前面的第一个p元素
$('div').next('p'); //选择div元素后面的第一个p元素
$('div').closest('form'); //选择离div最近的那个form父元素
$('div').parent(); //选择div的父元素
$('div').children(); //选择div的所有子元素
$('div').siblings(); //选择div的同级元素
$('div').find('.myClass'); //选择div内的class等于myClass的元素

```


jquery样式操作

jquery用法思想二
同一个函数完成取值和赋值

操作行间样式

```
// 获取div的样式
$("div").css("width");
$("div").css("color");

```
//设置div的样式

```
$("div").css("width","30px");
$("div").css("height","30px");
$("div").css({fontSize:"30px",color:"red"});

特别注意
选择器获取的多个元素，获取信息获取的是第一个，比如：$("div").css("width")，获取的是第一个div的width。

```

操作样式类名

```
$("#div1").addClass("divClass2") //为id为div1的对象追加样式divClass2
$("#div1").removeClass("divClass")  //移除id为div1的对象的class名为divClass的样式
$("#div1").removeClass("divClass divClass2") //移除多个样式
$("#div1").toggleClass("anotherClass") //重复切换anotherClass样式

```


jquery属性操作

1、html() 取出或设置html内容

// 取出html内容

> var $htm = $('#div1').html();

// 设置html内容

>$('#div1').html('<span>添加文字</span>');

2、text() 取出或设置text内容

```
// 取出文本内容

var $htm = $('#div1').text();

// 设置文本内容

$('#div1').text('<span>添加文字</span>');

```

3、attr() 取出或设置某个属性的值

```
// 取出图片的地址

var $src = $('#img1').attr('src');

// 设置图片的地址和alt属性

$('#img1').attr({ src: "test.jpg", alt: "Test Image" });


```


绑定click事件

给元素绑定click事件，可以用如下方法：

```
$('#btn1').click(function(){

    // 内部的this指的是原生对象

    // 使用jquery对象用 $(this)

})

```


jquery特殊效果

```
fadeIn() 淡入

    $btn.click(function(){

        $('#div1').fadeIn(1000,'swing',function(){
            alert('done!');
        });

    });

fadeOut() 淡出
fadeToggle() 切换淡入淡出
hide() 隐藏元素
show() 显示元素
toggle() 依次展示或隐藏某个元素
slideDown() 向下展开
slideUp() 向上卷起
slideToggle() 依次展开或卷起某个元素

```


jquery链式调用

jquery对象的方法会在执行完后返回这个jquery对象，所有jquery对象的方法可以连起来写：

```
$('#div1') // id为div1的元素
.children('ul') //该元素下面的ul子元素
.slideDown('fast') //高度从零变到实际高度来显示ul元素
.parent()  //跳到ul的父元素，也就是id为div1的元素
.siblings()  //跳到div1元素平级的所有兄弟元素
.children('ul') //这些兄弟元素中的ul子元素
.slideUp('fast');  //高度实际高度变换到零来隐藏ul元素

```



jquery动画

通过animate方法可以设置元素某属性值上的动画，可以设置一个或多个属性值，动画执行完成后会执行一个函数。

```
$('#div1').animate({
    width:300,
    height:300
},1000,swing,function(){
    alert('done!');
});

```
参数可以写成数字表达式：

```
$('#div1').animate({
    width:'+=100',
    height:300
},1000,swing,function(){
    alert('done!');
});
```

尺寸相关、滚动事件

1、获取和设置元素的尺寸

width()、height()    获取元素width和height  
innerWidth()、innerHeight()  包括padding的width和height  
outerWidth()、outerHeight()  包括padding和border的width和height  
outerWidth(true)、outerHeight(true)   包括padding和border以及margin的width和height

2、获取元素相对页面的绝对位置

> offse()

3、获取可视区高度

> $(window).height();

4、获取页面高度

> $(document).height();

5、获取页面滚动距离

```
$(document).scrollTop();  
$(document).scrollLeft();

```

6、页面滚动事件

```
$(window).scroll(function(){  
    ......  
})

```


jquery事件

事件函数列表：

blur() 元素失去焦点
focus() 元素获得焦点
change() 表单元素的值发生变化
click() 鼠标单击
dblclick() 鼠标双击
mouseover() 鼠标进入（进入子元素也触发）
mouseout() 鼠标离开（离开子元素也触发）
mouseenter() 鼠标进入（进入子元素不触发）
mouseleave() 鼠标离开（离开子元素不触发）
hover() 同时为mouseenter和mouseleave事件指定处理函数
mouseup() 松开鼠标
mousedown() 按下鼠标
mousemove() 鼠标在元素内部移动
keydown() 按下键盘
keypress() 按下键盘
keyup() 松开键盘
load() 元素加载完毕
ready() DOM加载完成
resize() 浏览器窗口的大小发生改变
scroll() 滚动条的位置发生变化
select() 用户选中文本框中的内容
submit() 用户递交表单
toggle() 根据鼠标点击的次数，依次运行多个函数
unload() 用户离开页面

绑定事件的其他方式

```
$(function(){
    $('#div1').bind('mouseover click', function(event) {
        alert($(this).html());
    });
});

```

取消绑定事件

```
$(function(){
    $('#div1').bind('mouseover click', function(event) {
        alert($(this).html());

        // $(this).unbind();
        $(this).unbind('mouseover');

    });
});

```



主动触发与自定义事件

主动触发
可使用jquery对象上的trigger方法来触发对象上绑定的事件。

自定义事件
除了系统事件外，可以通过bind方法自定义事件，然后用tiggle方法触发这些事件。

```

//给element绑定hello事件
element.bind("hello",function(){
    alert("hello world!");
});

//触发hello事件
element.trigger("hello");


```


事件冒泡

什么是事件冒泡
在一个对象上触发某类事件（比如单击onclick事件），如果此对象定义了此事件的处理程序，那么此事件就会调用这个处理程序，如果没有定义此事件处理程序或者事件返回true，那么这个事件会向这个对象的父级对象传播，从里到外，直至它被处理（父级对象所有同类事件都将被激活），或者它到达了对象层次的最顶层，即document对象（有些浏览器是window）。

事件冒泡的作用
事件冒泡允许多个操作被集中处理（把事件处理器添加到一个父级元素上，避免把事件处理器添加到多个子级元素上），它还可以让你在对象层的不同级别捕获事件。

阻止事件冒泡
事件冒泡机制有时候是不需要的，需要阻止掉，通过 event.stopPropagation() 来阻止

```

$(function(){
    var $box1 = $('.father');
    var $box2 = $('.son');
    var $box3 = $('.grandson');
    $box1.click(function() {
        alert('father');
    });
    $box2.click(function() {
        alert('son');
    });
    $box3.click(function(event) {
        alert('grandson');
        event.stopPropagation();

    });
    $(document).click(function(event) {
        alert('grandfather');
    });
})

......

<div class="father">
    <div class="son">
        <div class="grandson"></div>
    </div>
</div>

```

阻止默认行为
阻止右键菜单

```
$(document).contextmenu(function(event) {
    event.preventDefault();
});

```
合并阻止操作
实际开发中，一般把阻止冒泡和阻止默认行为合并起来写，合并写法可以用

```
// event.stopPropagation();
// event.preventDefault();

// 合并写法：
return false;
```




jquery元素节点操作

创建节点

```
var $div = $('<div>');
var $div2 = $('<div>这是一个div元素</div>');
```
插入节点

```
1、append()和appendTo()：在现存元素的内部，从后面插入元素

var $span = $('<span>这是一个span元素</span>');
$('#div1').append($span);
......
<div id="div1"></div>

```
> 2、prepend()和prependTo()：在现存元素的内部，从前面插入元素

> 3、after()和insertAfter()：在现存元素的外部，从后面插入元素

> 4、before()和insertBefore()：在现存元素的外部，从前面插入元素

删除节点

```
$('#div1').remove();

```
> todolist(计划列表)实例
