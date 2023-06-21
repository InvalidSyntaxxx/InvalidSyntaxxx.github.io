---
title: Less初学
tags: []
id: '1166'
categories:
  - - 文章
date: 2022-11-06 22:05:06
---

#### 变量

```less
@width: 10px;
@height:@width + 10px;

#header{
    width: @width;
    height: @height
}
```

#### 混合

```less
.bordered{
    border-top: dotted 1px black;
    border-bottom:solid 2px black;
}
#menu a {
    color: #111;
    .bordered();
}
.post a {
    color: red;
    .bordered();
}
```
<!-- more -->
#### 嵌套

```less
#header{
    color: black;
    .navigation {
        font-size: 12px;
    }
    .logo {
        width: 300px;
    }
}
```

等价于

```css
#header {
    color: black;
}
#header .navigation {
    font-size: 12px;
}
#header .logo {
    width: 300px;
}
```

#### @规则 嵌套和冒泡

```less
.component {
    width: 300px;
    @media (min-width: 768px) {
        width: 600px;
        @media (min-resolution: 192dpi) {
            background-image: url(/img/11.png);
        }
    }
    @media (min-width: 1280px) {
        width: 800px;
    }
}
```

等价于

```css
.component {
  width: 300px;
}
@media (min-width: 768px) {
  .component {
    width: 600px;
  }
}
@media (min-width: 768px) and (min-resolution: 192dpi) {
  .component {
    background-image: url(/img/retina2x.png);
  }
}
@media (min-width: 1280px) {
  .component {
    width: 800px;
  }
}
```

#### 运算\*

#### 转义

任何 `~"anything"` 或 `~'anything'` 形式的内容都将按原样输出，除非 [interpolation](https://less.bootcss.com/features/#variables-feature-variable-interpolation)。

```less
@min768: ~"(min-width: 768px)";
.element {
  @media @min768 {
    font-size: 1.2rem;
  }
}
```

编译为：

```less
@media (min-width: 768px) {
  .element {
    font-size: 1.2rem;
  }
}
```

注意，从 Less 3.5 开始，可以简写为：

```less
@min768: (min-width: 768px);
.element {
  @media @min768 {
    font-size: 1.2rem;
  }
}
```

#### 函数

[Less 函数手册](https://less.bootcss.com/functions/)

#### 命名空间和访问符

```less
#bundle(){
    .button{
        display: block;
        border: 1px solid black;
        &:hover {
            background-color: white;
        }
    }
    .tab {...}
    .citation {...}
}

#header a {
    color: orange;
    #bundle.button(); // 或者 #bundle > .button
}
```

如果不希望它们出现在输出的 CSS 中，例如 `#bundle .tab`，请将 `()` 附加到命名空间（例如 `#bundle()`）后面

#### 作用域

```less
@var: red;

#page {
  @var: white;
  #header {
    color: @var; // white
  }
}
/**
 两者一样
*/
@var: red;
#page {
  #header {
    color: @var; // white
  }
  @var: white;
}
```

#### 导入

```less
@import "library"; // library.less
@import "typo.css";
```