<center><h2>Sass常用到的语法</h2></center>

- #### 定义变量

  假设一个css文件中有统一的颜色配置，且好多处都被用到,这时候可以定义一个颜色变量

  ```scss
  	$color: 'red';
    body {
      color: $color
    }
  ```

- #### 支持嵌套

  在.css文件中，有如下代码, 三层代码，写起来比较麻烦

  ```css
  .wrapper {
      color: red;
  }
  .wrapper .content {
      color: black;
  }
  .wrapper .content .main {
      color: green;
  }
  ```

  但是，在.scss文件中，就省事很多了, 且层次分明， 他编译以后就是上边的代码

  ```scss
  .wrapper {
      color: red;
      .content {
          color: black;
          .main {
              color: green;
          }
      }
  }
  ```

- #### 引用父选择符

  再举个🌰， 在.css文件中，我们常会用到:after和:before 伪元素，例如有个title,他的前面又有一个小图标，这时如果不想再加一个dom元素，则可以用:befor伪元素，如下

  ```css
  .title {
      width: 100%;
      height: 35px;
  }
  .title:before {
      content: url('...');
  }
  ```

  在scss文件中

  ```
  .title {
      width: 100%;
      height: 35px;
      &:before {
          content: url('...')
      }
  }
  ```

- #### <span id = "mixin">混入</span> `@mixin` 

  先来认识一下混入的语法： `@mixin`  ,我所理解的混入主要是为了解决代码复用的问题。 它可以将一个css文件中很多重复使用的代码包装成一个混入对象，例如

  ```scss
   @mixin transformY ($value) {
       @if $value > 0 {
           transform: translateY(100% * $value - 50%);
       } @else {
           transform: translateY(100% * $value + 50%);
       }
       transition: all 3s;
  }
  ```

  先不用管里面的代码是如何实现的，后面会介绍到，上面的混入 transformY,主要执行的是 让元素沿着Y轴方向移动，特别说明一点，**混入是可以传参的**

- #### <span id='include'>引入混用样式</span> `@include`

  目前，我定义了如下几个类，都需要根据不同的参数来实现让元素在Y轴移动不同的位置，于是，我通过`@include`来引入,这样就避免了每个类都需要写上述的代码，所以，当动画移动规则发生变化的时候，我只需要修改 `mixin`就可以了。

  ```scss
  .box-animation-up1 {
      @include transformY(0);
  }
  .box-animation-up2 {
      @include transformY(-0.5);
  }
  .box-animation-up3 {
      @include transformY(-1);
  }
  .box-animation-up4 {
      @include transformY(-1.5);
  }
  .box-animation-up5 {
      @include transformY(-2);
  }
  ```

- #### `@if ... @else`

  在所有的语言中，基本上都有 if…else的语法，Sass也不列外，意义和其他语言是完全一样的。

  还是拿[混入](#mixin)的代码来举例。

  ```scss
   @mixin transformY ($value) {
       @if $value > 0 {
           transform: translateY(100% * $value - 50%);
       } @else {
           transform: translateY(100% * $value + 50%);
       }
       transition: all 3s;
  }
  ```

  上述代码就是通过 if...else 来实现的， 通过判断传出的参数 是大于0 还是小于0，执行不同的代码。

- #### `@for`

  同理，语法相信所有人都知道。不知道看了上面[引入混用样式](#include)的部分有没有感想，我当时就是觉得这样写代码太烂了，感觉只是类名尾部的数字不一样， 传参不一样，其他地方完全一样，想想可以用for循环解决啊，所以就有了如下代码。

  ```scss
   @for $i from 1 through 5 {
       .box-animation-up#{$i} {
           @include transformY( -($i - 1)/2)
       }
  }
  ```

  Sass 还有好多有用的语法没有用到，等后续用到了以后再来更新此篇文章。:happy:
