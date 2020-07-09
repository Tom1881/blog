# Container

## 简介

- 单容器类Widget，即只包含一个子Widget；
- 装饰子Widget，例如设置背景颜色、形状；
- 定位子Widget；

## 属性

- child属性，放置子Widget

  ```dart
  child: Text("三国演义"),
  ```

- color属性，给子Widget设置背景色

  ```dart
  color: Colors.blue,
  ```

- width和height属性，给子Widget设置宽高，如果不设置，Contaniner会根据子Widget自行调整大小

  ```dart
  width: 100,
  height: 100,
  ```

- padding属性，内边距，Container和子Widget之间的距离

  ```dart
  padding: EdgeInsets.all(20),
  ```

- margin属性，外边距，Contaniner和父Widget之间的距离

  ```dart
  margin: EdgeInsets.all(40),
  ```

- decoration属性，装饰，和Container的color属性互斥

  ```dart
  decoration: BoxDecoration(
       shape: BoxShape.circle,//圆形背景
       color: Colors.pink,
  ),
  ```

- decoration属性进阶

  ```dart
  decoration: BoxDecoration(
    shape: BoxShape.rectangle, //背景形状
    borderRadius: BorderRadius.all(Radius.circular(30)), //圆角的半径
    color: Colors.pink, //背景的颜色
    border: Border.all(
      color: Colors.blue,
      width: 2,
    ),
  ),
  ```

- decoration属性进阶实现圆角图片和圆形图片

  ```dart
          width: 200,
          height: 200,
          decoration: BoxDecoration(
              image: DecorationImage(
                image: NetworkImage(
                  "https://flutter.github.io/assets-for-api-docs/assets/widgets/owl-2.jpg",
                ),
                fit: BoxFit.fill,
              ),
              border: Border.all(
                color: Colors.blue,
                width: 5,
              ),
              //borderRadius: BorderRadius.circular(12),//圆角，
              shape: BoxShape.circle), //和borderRadius互斥
  ```

- alignment对齐属性，设置了此属性时如果没有给Container设置width和height，那么Container会充满其父Widget

  ```dart
  alignment: Alignment.bottomCenter,
  ```

- constraints约束尺寸属性，默认最小是0，最大是double.infinity无限大

  ```dart
          constraints: BoxConstraints.tightForFinite(
            width: double.infinity,
            height: 20,
          ),
  ```

- transform属性，转换，可以旋转，平移和缩放

  ```dart
  transform: Matrix4.rotationZ(0.5),//弧度，不是角度
  ```


## 参考资料

[老孟Flutter](http://laomengit.com/flutter/widgets/Container.html)

