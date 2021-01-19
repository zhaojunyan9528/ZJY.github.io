---
title: css-grid布局
tags:
  - 前端
categories:
  - - 笔记
    - css
date: 2021-01-19 16:52:37
---

Flex是轴线布局，只能指定“项目” 针对轴线的位置，可以看作是一维布局。

Grid布局则是将容器分成行和列，产生单元格，然后指定项目所在单元格，可以看作是二维布局

### 1.容器和项目：

采用网格布局的区域，称为“容器” container， 容器内采用网格定位的子元素，称为“项目” item

```html
<div>
    <div>
        <p>1</p>
    </div>
    <div>
        <p>2</p>
    </div>
    <div>
        <p>3</p>
    </div>
</div>
```

最外层的div元素就是容器， 内层的三个div 是项目
注意： 项目只能是容器的顶层子元素，不包含项目的子元素，grid布局只对项目生效

### 2.行和列

容器里水平区域称为"行"row,垂直区域称为“列”column

### 3.单元格    

行和列的交叉区域，称为“单元格”cell
正常情况下，N行M列，会产生N*M个单元格

### 4.网格线    

划分网格的线，称为"网格线"（grid line）。水平网格线划分出行，垂直网格线划分出列
正常情况下，n行有n + 1根水平网格线，m列有m + 1根垂直网格线

Grid 布局的属性分成两类。一类定义在容器上面，称为容器属性；另一类定义在项目上面，称为项目属性。这部分先介绍容器属性

### 5.容器属性

#### 5.1 display属性

display: grid指定一个容器采用网格布局

```css
    div {
        display: grid;
    }
```

默认情况下，容器元素都是块级元素，但也可以设成行内元素

```css
    div {
        display: inline-grid;
    }
```

注意，设为网格布局以后，容器子元素（项目）的float、display: inline-block、display: table-cell、vertical-align和column-*等设置都将失效。

#### 5.2 grid-template-rows属性和grid-template-columns属性

容器指定了网格布局以后，接着就要划分行和列。grid-template-columns属性定义每一列的列宽，grid-template-rows属性定义每一行的行高。

```css
    .container {
        display: grid;
        grid-template-columns: 100px 100px 100px;
        grid-template-rows: 100px 100px 100px;
    }
```

除了使用绝对单位,还可使用百分比：

```css
grid-template-columns: 33.33% 33.33% 33.33%;
grid-template-rows: 33.33% 33.33% 33.33%;
```

1.repeat() 接受两个参数，第一个参数是重复的次数,第二个参数是所要重复的值:

```css
    grid-template-columns: repeat(3, 33.33%)
```

重复某种模式：定义了6列，第一列和第四列的宽度为100px，第二列和第五列为20px，第三列和第六列为80px

```css
    grid-template-columns: repeat(2, 100px 20px 80px)
```

2.auto-fill关键字
    有时，单元格的大小是固定的，但是容器的大小不确定。如果希望每一行（或每一列）容纳尽可能多的单元格，这时可以使用auto-fill关键字表示自动填充

```css
    grid-template-columns: repeat(auto-fill, 100px)
```

表示每列宽度100px，然后自动填充，直到容器不能放置更多的列

3.fr关键字

为了方便表示比例关系，网格布局提供了fr关键字（fraction 的缩写，意为"片段"）。如果两列的宽度分别为1fr和2fr，就表示后者是前者的两倍

```css
    grid-template-columns: 1fr 1fr
```

表示两个相同宽度的列。
fr可以与绝对长度的单位结合使用，这时会非常方便

```css
    grid-template-columns: 150px 1fr 2fr
```

表示第一列宽度未150像素，第二列宽度是第三列的一半

4.minmax()函数产生一个长度范围，表示长度就在这个范围内，接收两个参数，分别为最小值和最大值

```css
    grid-template-columns: 1fr 1fr minmax(100px, 1fr);
```

5.auto关键字表示由浏览器自己决定长度

```css
    grid-template-columns: 100px auto 100px;
```

第二列的宽度，基本上等于该列单元格的最大宽度，除非单元格内容设置了min-width，且这个值大于最大宽度

6.网格线的名称

grid-template-columns属性和grid-template-rows属性里面，还可以使用方括号，指定每一根网格线的名字，方便以后的引用。

```css
    grid-template-columns: [c1] 100px [c2] 100px [c3] auto [c4];
    grid-template-rows: [r1] 100px [r2] 100px [r3] auto [r4]
```

7.布局实例

```css
    grid-template-columns: 70% 30%;
```

上面代码将左边栏设为70%，右边栏设为30%。
传统的十二网格布局，写起来也很容易。

```css
    grid-template-columns: repeat(12, 1fr);
```

#### 5.3 grid-row-gap属性 grid-column-gap属性 grid-gap属性

grid-row-gap属性设置行与行的间隔（行间距），grid-column-gap属性设置列与列的间隔（列间距）。

```css
    grid-row-gap: 20px;
    grid-column-gap: 20px;
    grid-gap: <grid-row-gap> <grid-column-gap>
    grid-gap:20px; /* 如果省略了第二个值，则默认第二个值等于第一个值 */
```

注： 根据最新标准，上面三个属性名的grid-前缀已经删除，grid-column-gap和grid-row-gap写成column-gap和row-gap，grid-gap写成gap。

### 5.4 grid-template-areas属性

网格布局允许指定"区域"（area），一个区域由单个或多个单元格组成。grid-template-areas属性用于定义区域。

```css
    grid-template-areas: 'a b c'
                       'd e f'
                       'g h i';
```

上面代码先划分出9个单元格，然后将其定名为a到i的九个区域，分别对应这九个单元格。
多个单元格合并成一个区域的写法如下：

```css
    grid-template-areas: 'a a a'
                     'b b b'
                     'c c c';
```

上面代码将9个单元格分成a、b、c三个区域。
如果某些区域不需要利用，则使用"点"（.）表示

```css
    grid-template-areas: 'a . c'
                     'd . f'
                     'g . i';
```

注意，区域的命名会影响到网格线。每个区域的起始网格线，会自动命名为区域名-start，终止网格线自动命名为区域名-end。

### 5.5 grid-auto-flow属性

划分网格以后，容器的子元素会按照顺序，自动放置在每一个网格。默认的放置顺序是"先行后列"，即先填满第一行，再开始放入第二行，即下图数字的顺序。
这个顺序由grid-auto-flow属性决定，默认值是row，即"先行后列"。也可以将它设成column，变成"先列后行"。
grid-auto-flow: row/row dense/column/column dense

### 5.6 justify-items属性 align-items属性 place-items属性

justify-items属性设置单元格内容的水平位置（左中右），align-items属性设置单元格内容的垂直位置（上中下）

```css
    justify-items: start | end | center | stretch;
    align-items: start | end | center | stretch;
        start：对齐单元格的起始边缘。
        end：对齐单元格的结束边缘。
        center：单元格内部居中。
        stretch：拉伸，占满单元格的整个宽度（默认值）。
    place-items属性是align-items属性和justify-items属性的合并简写形式。
```

#### 5.7 justify-content属性 align-content 属性 place-content 属性 

justify-content属性是整个内容区域在容器里面的水平位置（左中右），align-content属性是整个内容区域的垂直位置（上中下）。

```css
justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
align-content: start | end | center | stretch | space-around | space-between | space-evenly; 
```

### 5.8 grid-auto-rows 属性和 grid-auto-columns属性

有时候，一些项目的指定位置，在现有网格的外部。比如网格只有3列，但是某一个项目指定在第5行。这时，浏览器会自动生成多余的网格，以便放置项目
grid-auto-columns属性和grid-auto-rows属性用来设置，浏览器自动创建的多余网格的列宽和行高。它们的写法与grid-template-columns和grid-template-rows
完全相同。如果不指定这两个属性，浏览器完全根据单元格内容的大小，决定新增网格的列宽和行高。

```css
    grid-auto-rows: 50px;
    .item8{
        grid-row-start: 4;
    }
```

指定第8个项目位于第四行行高50像素

### 6.项目属性

#### 6.1 grid-column-start grid-column-end grid-row-start grid-row-end

项目的位置是可以指定的，具体方法就是指定项目的四个边框，分别定位在哪根网格线。

    grid-column-start属性：左边框所在的垂直网格线
    grid-column-end属性：右边框所在的垂直网格线
    grid-row-start属性：上边框所在的水平网格线
    grid-row-end属性：下边框所在的水平网格线

```css
    .item-1 {
        grid-column-start: 2;
        grid-column-end: 4;
    }
```

1号项目的左边框是第二根垂直网格线，右边框是第四根垂直网格线。
这四个属性的值，除了指定为第几个网格线，还可以指定为网格线的名字。
这四个属性的值还可以使用span关键字，表示"跨越"，即左右边框（上下边框）之间跨越多少个网格。

```css
    grid-column-start: span 2;
```

#### 6.2 grid-column 属性 grid-row 属性

grid-column属性是grid-column-start和grid-column-end的合并简写形式，grid-row属性是grid-row-start属性和grid-row-end的合并简写形式。

    grid-column: 1/3
    grid-column 1/span 2
    占据两列

#### 6.3 grid-area

grid-area属性指定项目放在哪一个区域

```css
    .container{
        grid-template-areas: 'a b c'
                                'd e f'
                                'g h i'
    }
    .item1{
        grid-area :e
    }
```

#### 6.4 justify-self 属性，align-self 属性，place-self 属性   

justify-self属性设置单元格内容的水平位置（左中右），跟justify-items属性的用法完全一致，但只作用于单个项目。
align-self属性设置单元格内容的垂直位置（上中下），跟align-items属性的用法完全一致，也是只作用于单个项目。
