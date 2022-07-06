sass基本用法

一.sass中定义变量用$
$widths:200px;

.box{
  width：$widths
}

$widths:200px !default;
.box{
	width:$widths
}

变量镶嵌在字符串中 写在#{}之中
局部变量在局部使用，全局变量在任意地方使用

二.嵌套
ul{
	li{
		color:red;
		&:hover a{
			color:#000;
		}
	}
}

&代表父级

三.混合宏

1.声明与引入
@mixin声明   @include引入

@minxin rounded-corners{
	 ...
	 ...
	 ...
}

notice{
  background-color: green;
  border: 2px solid #00aa00;
  @include rounded-corners;
}

2.混合器中不仅可以包含属性，也可以包含css规则，包含选择器和选择器中的属性，

@mixin no-bullets {
  list-style: none;
  li {
    list-style-image: none;
    list-style-type: none;
    margin-left: 0px;
  }
}

ul.plain {
  color: #444;
  @include no-bullets;
}

变为：
ul.plain {
  color: #444;
  list-style: none;
}
ul.plain li {
  list-style-image: none;
  list-style-type: none;
  margin-left: 0px;
}

3.混合器传参
@mixin link-colors($normal, $hover, $visited) {
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}

a {
  @include link-colors(blue, red, green);
}

4.默认参数值
@mixin link-colors(
    $normal,
    $hover: $normal,
    $visited: $normal
  )
  
{
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}

四.选择器继承
//通过选择器继承继承样式
.error {
  border: 1px solid red;
  background-color: #fdd;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}

.seriousError不仅会继承.error自身的所有样式，任何跟.error有关的组合选择器样式也会被.seriousError以组合选择器的形式继承，如下代码:

//.seriousError从.error继承样式
.error a{  //应用到.seriousError a
  color: red;
  font-weight: 100;
}
h1.error { //应用到hl.seriousError
  font-size: 1.2rem;
}

五.导入其他文件

@import "xxx"  //可以省略后缀，文件只有在执行到@import时才会下载其他css文件，导致变慢