# Column

## 简介

- 多子Widget的容器类控件；
- Row控件水平布局；
- Column垂直布局；
- mainAxisAlignment主轴，和容器方向一致的轴，例如Row的为水平方向；
- crossAxisAlignment交叉轴，和容易方向垂直的轴，例如Row的为垂直方向；

## 属性

- 主轴对齐方式mainAxisAlignment，此属性最终效果还受textDirection的影响
  - MainAxisAlignment.start：从开始位置排列，开始位置不一定是左边；
  - MainAxisAlignment.center：排列在中间；
  - MainAxisAlignment.end：从结束位置排列，结束位置不一定是右边；
  - MainAxisAlignment.spaceBetween：开始和结束位置没有空白，其余间距平分；
  - MainAxisAlignment.spaceEvenly：开始和结束也平分间距；
  - MainAxisAlignment.spaceAround：开始和结束间距是其余间距的一半；
- 交叉轴对齐方式
  - 默认是居中；
  - CrossAxisAlignment.start：这个start指的是交叉轴的start，如果Row/Column没有高度限制，交叉轴长度等于最高的子Widget高度；
  - CrossAxisAlignment.center：交叉轴的中央部分，如果Row/Column没有高度限制，交叉轴长度等于最高的子Widget高度；
  - CrossAxisAlignment.end：交叉轴的末尾部分，如果Row/Column没有高度限制，交叉轴长度等于最高的子Widget高度；
  - CrossAxisAlignment.stretch：拉伸子Widget填满Row/Column的交叉轴，此时交叉轴的长度等于Row/Column的高度；
- textDirection/verticalDirection
  - textDirection只对Row生效；
  - verticalDirection只对Column生效；
- 主轴尺寸mainAxisSize
  - MainAxisSize.max：主轴尽可能的大；
  - MainAxisSize.min：主轴尽可能的小，只要包裹住子Widget即可。

## 参考资料

[老孟Flutter](http://laomengit.com/flutter/widgets/Column.html)