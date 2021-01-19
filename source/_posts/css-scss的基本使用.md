---
title: css-scss的基本使用
tags:
  - 前端
categories:
  - - 笔记
    - css
date: 2021-01-19 22:10:39
---

### scss基础使用

#### 变量:

```css  
    $primary-color: #341234;

    div.box{
        background-color: $primary-color;
    }
    nav{
        border: 1px solid #000 {
            left:0;
            right:0;
        }
    }
```

#### 混合属性:

```css
    @mixin  alert {
        color: #723899;
        background-color: $primary-color;
    }
    @mixin  alert-a {
        color: #723899;
        background-color: $primary-color;
        a{
            color:$primary-color;
        }
    }

    @mixin alert-b($text-color,$background) {
        color: $text-color;
        background-color: $background;
        span{
            color: darken($text-color,10%);
        }
    }

    ul{
        @include alert
    }
    li{
        @include alert-a;
    }

    a{
        @include alert-b(#786789,#000 );
    }
```

#### 继承:

```css
    .alert {
        padding: 15px;
    }

    .alert-info {
        @extend .alert;
        background: #fff;
    }
```

#### 颜色:

rgb(255,255,255)
rgba(255,255,255,1)
红绿蓝透明度

hsl(50,100%,50%)
hsla(50,100%,50%,0.5)
色相 饱和度 明度 透明度

adjust-hue调整颜色 background: adjust-hue(#fff, 123deg)

lighten darken函数 $lighten-color: lighten(#fff,30%) $darken-color: darken(#fff,20%)

saturate增加颜色的纯度 desaturate减少颜色的纯度
$saturate-color: saturate(#000,40%)
$desaturate-color: desaturate(#000,40%)

opacify添加透明度(让颜色更不透明)  transparentize减少透明度(让颜色更透明)

#### interpolation插入值 :

```css
    $text: 'info';
    .alert-#{$text} {
        background:$primary-color;
    }
    results:
    .alert-info{
        background: #fff;
    }
```

#### if条件判断:

```css
    @if condition{
        ...
    }

    $conditon: 2 > 1;
    .rounded {
        @if $conditon {
            -webkit-animation-name: taro;
            -moz-animation-name: taro;
        }
        border-radius: 5px;
    }

    $color : "dark";
    body{
        @if $color == dark {
            background-color: darkblue;
        } @else if $color == lighten {
            background-color: lightblue;
        } @else {
            background: gray;
        }
    }
```

#### for循环:

```css
    @for $var from <begain> through <end> {
        ...
    }

    $columns: 4;
    @for $i from 1 through $columns {
        .col-#{$i} {
            width: 100% / $columns * $i;
        }
    }
    results:
    .col-1 {
        width: 25%; }
    
    .col-2 {
        width: 50%; }
    
    .col-3 {
        width: 75%; }
    
    .col-4 {
        width: 100%; }

    @for $i from 1 to $columns {
        .col-#{$i} {
            width: 100% / $columns * $i;
        }
    }
    results:
    .col-1 {
        width: 25%; }
    
    .col-2 {
        width: 50%; }
    
    .col-3 {
        width: 75%; }
```

#### each语句

```css
    @each $var in $lists {
        ...
    }

    $icons: success error warning;
    @each $icon in $icons {
        .icon-#{$icon}{
            background-image: url(../image/icons/#{$icon}.png);
        }
    }
    results:
    .icon-success {
        background-image: url(../image/icons/success.png); }
    
    .icon-error {
        background-image: url(../image/icons/error.png); }
    
    .icon-warning {
        background-image: url(../image/icons/warning.png); }
```

#### while循环语句

```css
    @while condition {
        ...
    }

    $i : 6;
    @while $i > 0 {
        .item-#{$i} {
            width: 5px * $i;
        }
        $i : $i - 2;
    }
    results:
    .item-6 {
        width: 30rpx; }
    
    .item-4 {
        width: 20rpx; }
    
    .item-2 {
        width: 10rpx; }
```

#### 用户自定义函数:

```css
    $colors: (light: #fff, dark: #000);
    @function color($key){
        // @if not map-has-key($colors, $key){
        //     @warn "在colors中没有这个属性"
        // }
        @return map-get($colors,$key);
    }
    .test {
        background-color: color(gray);
    }
    results:
        .test {
            background-color: #fff;
        }
```

#### @warn 警告信息  @error错误信息

#### sass编译输入四种格式:

1.nested,嵌套
2.compact,紧凑
3.expander,扩展
4.compressed,压缩

.sass缩进式写法  .scss嵌套写法

#### partials与@import:

文件名以_下划线开头的是partials,不会编译 _base.scss
可以通过@import "base";将文件引入