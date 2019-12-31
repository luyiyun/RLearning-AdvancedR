Introduction
================
荣志炜
2019/12/29

  - [1. 介绍](#介绍)
  - [2. OOP系统](#oop系统)
  - [3. R中的OOP](#r中的oop)
  - [4. sloop包](#sloop包)

## 1\. 介绍

这一部分主要介绍R中的OOP，即面向对象编程。学习R中的OOP是有点难度的，原因有如下几点:  
1\. R中有好几个OOP系统：S3、R6、S4，还有一个不常用的RC；  
2\. 这几个OOP系统到底谁最重要是存在分歧的，Hadley本人认为S3\>R6\>S4；  
3\. S3和S4是泛型函数（generic function）OOP，这和其他语言常用的封装OOP（encapsulate OOP）不一样。

总的来说，在R中fp（函数式编程）要比OOP更加重要。但各个OOP系统也有以下学习的必要：  
1\. S3使得在base R中可以让我们的函数得到更加丰富的结果，针对不同的输入；  
2\. R6提供了一个避免copy-on-modify的方式，这在web API编程的时候很重要；  
3\. S4是一个比S3更加严格的系统，这使得我们在开发大型程序、多人协作开发的时候受益匪浅，这也是Bioconductor使用S4的原因。

## 2\. OOP系统

使用OOP系统的一大原因是OOP系统提供了多态性（polymorphism），就是使用相同的函数根据不同类型的输入来得到完全不同的输出，比如下面的例子：

``` r
diamonds <- ggplot2::diamonds

summary(diamonds$carat)
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##  0.2000  0.4000  0.7000  0.7979  1.0400  5.0100
summary(diamonds$cut)
##      Fair      Good Very Good   Premium     Ideal 
##      1610      4906     12082     13791     21551
```

这和if-else不同：如果使用if-else，如果我们希望增加一个新的类型到`summary`函数的实现中时，我们必须去更改`summary`函数本身；而OOP系统则使得我们不必更改`summary`本身就可以实现，这使得任何开发者都可以做到。

这里明确几个在OOP系统中比较常用的概念：  
1\. class，类；  
2\. method，方法；  
3\. field，实例拥有的数据；  
4\. inherit，继承；  
5\. method dispath，为给定的类找到其正确的方法的过程。

## 3\. R中的OOP

Base R中提供了以下3种OOP系统：  
\- **S3**是R的第一个OOP系统，非正式实现，通常依赖于约定而没有严格的规范，比较适合快速实现一些简单的任务；  
\- **S4**是S3的正式的、严格的改写，比S3需要更多的工作，但保证了严格和封装性，在**methods**包中提供；  
\-
**RC**实现了封装OOP，是S4类型的特殊形式，是可变的（mutable），即没有copy-on-modify，但这也是的其比较难以解释。

CRAN的包还提供了以下3种其他的OOP系统：  
\- **R6**提供了类似RC的封装OOP系统，但解决了RC存在的一些问题；  
\- **R.oo**提供了S3一些形式上的规定，使得S3也可以mutable；  
\-
**proto**提供了另外的基于**原型（prototypes）**的OOP系统，其模糊了类和实例的界限，并在ggplot2中有一定的使用，但Hadley认为还是用标准的形式好。

## 4\. sloop包

sloop（sail the seas of
OOP）将提供一些方法来帮助我们理解OOP系统。比如，使用`sloop::otype()`将提供这些对象使用的OOP系统：

``` r
sloop::otype(1:10)
## [1] "base"
sloop::otype(mtcars)
## [1] "S3"
mle_obj <- stats4::mle(function(x = 1) (x - 2) ^ 2)
sloop::otype(mle_obj)
## [1] "S4"
```
