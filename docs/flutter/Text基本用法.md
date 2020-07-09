# Text

## 简介

- 显示文本的组件

## 属性

- 设置文本

  ```dart
  Text("三国演义"),
  ```

- style，样式，可以设置颜色，字体，大小，背景颜色，值为TextStyle

  ```dart
  color: Colors.pinkAccent,
  fontSize: 16,
  fontWeight: FontWeight.bold,
  ```

- textAlign,对齐方式，值为TextAlign的静态变量

  ```dart
  textAlign: TextAlign.center,
  ```

- textDirection文本排列方向，值为TextDirection的静态值，和textAlign互斥

  ```dart
  textDirection: TextDirection.rtl,
  ```

- softWrap是否换行，默认true，设置为false时，如果文本过长直接截断

  ```dart
  softWrap: false,
  ```

- overflow溢出的处理方式，值为TextOverflow的静态值

  ```dart
  overflow: TextOverflow.visible,
  ```

- textScaleFactor文字缩放系数

  ```dart
  textScaleFactor: 1.5,//是原来的1.5倍
  ```

## 参考资料

[老孟Flutter](http://laomengit.com/flutter/widgets/Text.html)