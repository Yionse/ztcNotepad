# 基础部分

## 常用的属性前缀

- -webkit：webkit核心浏览器，包括Chrome、Safari
- -moz：火狐(Firefox)
- -ms：IE浏览器
- -o：Opera浏览器

## 浮动

- 对于块级元素来说，在不设置宽度的情况下，默认的宽度是100%，一旦设置了浮动它的宽度就会根据内容自动调整
- 设置了浮动的元素，将会脱离文档流

## 清除浮动

- 清除浮动可以让元素既浮动，但是又存在于盒子中，不脱硫文档流

- 清除浮动的三种方式

  - 给父盒子添加高度，但有时候并不需要给盒子添加高度，而是父盒子自适应高度，这种方式不推荐

  - 额外标签法，在浮动元素的后面添加一个块级元素，并添加clear：both；属性，这样就可以成功清除浮动。但也不推荐，因为这样会破坏HTML结构

  - 给父元素添加overflow：hidden；但是本身的意思是溢出的内容隐藏，可以视情况去用

  - 添加::after伪元素的方法，也可以选择使用

    ```js
    .clearfix::after {
        content: '';
        height: 0;
        clear: both;
        visibility: hidden;
        display: block;
    }
    .clearfix {
        *zoom: 1;
    }
    ```

    

  - 添加双伪元素，可以添加双伪元素也可以添加单伪元素，最常用的方法

    ```js
    .clearfix::after,
    .clearfix::before {
        content: '';
        display: table;
    }
    .clearfix::after {
        clear: both;
    }
    .clearfix {
        *zoom: 1;	//	照顾低版本浏览器
    }
    ```

  ## 定位

  - relative和static都是相对定位，也就是属于标准流，但relative可以通过top和left等移动位置，static不可以 
  - 设置了定位的块级盒子如果没有设置宽度，则根据其内容调整，如果设置为relative和static则还是默认宽度100%

  ## 反射——box-reflect

  - {<方向><间距><渐变效果>}
    - 方向：above、below、left、right
    - 间距：表示倒影和元素之间的距离，padding会产生影像
    - 渐变：<url>填充图片、<linear-gradient>线性渐变

#   弹性布局

-    flex-direction：设置主轴的方向
     -    row，默认值，以水平方向为主轴
     -    row-reverse，元素的显示顺序从右至左
     -    column，以垂直方向为主轴
     -    column-reverse，元素的显示顺序相反
-    flex-wrap：设置容器是否换行
     -   no-wrap：项目自动收缩，不允许换行显示
     -   warp：允许换行显示
-    flex-flow：direction和wrap的组合属性，第一个值为主轴方向，第二个值为换行显示
-    justify-content：设置主轴项目的排列方式
     -   flex-start：与主轴起始对齐，默认
     -   flex-end：与主轴终点对齐
     -   center：居中对齐
     -   以上三个属性都是整体进行排列
     -   space-between：两端对齐，首位的项目紧贴容器，剩余空间平均分配
     -   space-around：分散对齐，每个项目两边的距离都是相等的，头尾项目的空间是中间项目的1/2
     -   space-evenly：平均对齐，每个项目两边的距离都相等
-    align-itmes：设置交叉轴的项目
     -   flex-start：与交叉轴起始对齐，默认
     -   flex-end：与交叉轴终点对齐
     -   center：居中对齐
-    align-content：多行容器中所有项目在交叉轴的对齐方式
     -   stretch：默认，项目拉伸至整个交叉轴
     -   flex-start：与交叉轴起始对齐，默认
     -   flex-end：与交叉轴终点对齐
     -   以上三种是整体对齐
     -   space-between：两端对齐
     -   space-around：分散对齐，每个项目两边的距离都是相等的，头尾项目的空间是中间项目的1/2
     -   space-evenly：平均对齐，每个项目两边的距离都相等
-    align-self：单个项目的对齐方式
     -   auto：继承align-items中的值
     -   flex-start：与交叉轴起始对齐
     -   flex-end：与交叉轴终点对齐
     -   center：居中对齐
     -   stretch：在交叉轴方向上拉伸
-    order：设置单个项目的排列顺序
     -   0：默认值，按书写顺序显示
     -   n：n越小越靠前
-    flex-grow：设置项目在主轴上的放大因子
     -   0：默认值
     -   n：放大的因子，即背数，必须为正整数
-    flex-shrink：设置项目在主轴上的缩小因子，当主轴的空间容纳不下所有项目时，才有意义
     -   1：允许主轴上的项目自适应
     -   0：不缩小
     -   n：缩小因子，正整数
-    flex-basis：设置项目在主轴上的尺寸，如果存在最小宽度，则以最小宽度为准，项目的尺寸优先级从高往低排列依次为最小宽度，flex.basis，以及项目原本设置的宽高
     -   auto：默认值，项目原来的大小
     -   px：像素，会覆盖项目一开始设置的大小
     -   %：百分比，相对于父容器而言
-    flex：flex-grow，flex-shrink，flex-basis三个值的缩写
     -   三值：三个值都有
     -   双值：第一个值是放大因子，第二个值是基准尺寸
     -   单值：

