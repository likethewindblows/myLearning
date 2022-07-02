LESS学习
一.注释

除了css注释外 less支持 //注释

二.变量
1:属性中使用变量：

声明：
@myWidth:10px;
使用：
#header{
 width:@myWidth
}
2.选择器中使用变量

声明：
@my-selector:banner
使用：
.@{my-selecor}{
width:100px
}
3.在url中使用变量

@images:"../img"

body{
	background: :url("@{images}/white-sand.png");
}

三.作用域

@var:red;

@page{
	@var:white;
	@header{
		color:@var //white 沿着作用域链查找
	}
}

四.导入外部less(@import)

@import "lib.less"
@import "lib"


五.嵌套
//嵌套会将父级和祖级元素添加到前面

#header{
	color:black;
	.logo{  // #header .logo
		color:red;
		tr{  //#header .logo tr
			color:yellow;
		}
	}
}

六.父级选择器
LESS通过&简写父级选择器

a{
	color:blue;
	&:hover{   //等同于 a:hover
		color:green;
	}
	
	&+&{ //a+a
		
	}
	
	&-children{ //a-children
		
	}
}

七.冒泡
//针对媒体查询，这样同样的样式就不用复写了。
.component {
    width:300px;
    color:white;
    @media (min-width: 768px) {  // 等于复写一个@media>.component，已有的color属性将写入，width将覆盖
        width:600px;
    }
}

八.运算
LESS支持+ - * / 基本同JS
属性值的单位将参与换算  12px+12px = 24px
单位无法换算 将会取消单位计算 12em+12px = 24

九.转义
转义中的文字将会原样输出,不会进行计算和转化

@data:~"min-width:640px"

@min768:(min-width:768px)

十.语言拓展

1.Mixins混入样式

.borderd{
	border-top:2px
}
混入:
#mian a{
	color:white;
	.borderd() //border-top:2px
}

2.带参数的混入样式

.border-radius(@radius){
	border-radius:@radius
}
调用
header{
	.border-dadius(4px);//border-radius:4px
}

3.参数可带默认值,可多个,可覆盖,和@arguments
.border-radius(@radius:5px,@clolor:yellow){
	//@arguments 变量包含所有参数的值
}
调用
header{
	.border-radius;//直接调用
	.border-radius(2px) //部分覆盖
}
4.不带参数(隐藏混入)
// 集合将被隐藏,不会暴露到css中
.wrap(){
	text-wrap:wrap
}
//调用
pre{
	.wrap
}

十一.命名空间
命名空间用于在某一样式下声明的mixin 可以和其他mixin区分开

.bundle{
	mixin(){
		
	}
}
调用

.bundle>mixin()

十二.模式匹配的混合样式
//可以给多个样式匹配同一指定

.mixin (dark, @color) {  //只接受dark为首参
  color: darken(@color, 10%);
}
.mixin (light, @color) {  //只接受light为首参
  color: lighten(@color, 20%);
}
.mixin (@_, @color) {  //接受任意值
  display: block;
}

十三.样式继承

.inline{
	width:100px
}

nav ul{
	&:extend(.inline)
	color:white
}

十四.样式合并
1.逗号合并(使用+)

.myfunc(){
	box-shadow+:5px 5px 5px grey
}

.class{
	 .myfunc()
	 box-shadow+:0 0 5px #f78181
}
//结果 box-shadow : 5px 5px 5px grep , 0 0 5px #f78181

2.空格合并(使用+ 或者+_)
.mixin(){
	transform+_:scale(1)
}
.class{
	.mixin();
	transform+_:rotate(2deg)
}
// 结果 transform:scale(1) rotate(2deg)


十五.导引
//类似条件判断 LESS通过导引混合而非if/else语句来实现条件判断，因为前者已在@media query特性中被定义。

.mixin(@a) when(lightness(@a) >= 50%){//后面的运算为true值，则应用样式
	background-color: black;
}

.mixin(#ddd)
//导引中可用的全部比较运算有： >， >=， =， =<， <。，
//注意a=b，等同于js中a==b
//此外，关键字true只表示布尔真值,除去关键字true以外的值都被视示布尔假
//使用not关键字表示非


//导引序列使用逗号‘,’—分割,表示&&
.mixin (@a) when (@a > 10), (@a < -10) { ... }

//参数比较运算
.max (@a, @b) when (@a > @b) { width: @a }


//is*函式检验是否为某数据类型
.mixin (@a) when (isnumber(@a)) and (@a > 0) { ... }


//CSS Guards(略)
.style when (@usedScope=mixin)

十六.循环

.cont(@count) when(@count>0){
	.cont((@count -1));
	width:(25px * @count)
}
div{
	.cont(7)
}

十七.映射
//从 Less 3.5 版本开始，你还可以将混合（mixins）和规则集（rulesets）作为一组值的映射（map）使用。
#colors(){
	primart:blue;
	secondary: green;
}
.button {
  color: #colors[primary];
  border: 1px solid #colors[secondary];
}

十八.JavaScript 表达式引入
LESS引入JS很简单，只需要反引号

//使用反引号表示
@var: `"hello".toUpperCase() + '!'`;

十九.内置函数
https://less.bootcss.com/functions/#logical-functions

@width: 0.5;

percentage(@width); //50%