# Image和Icon

## 简介

- 本质上Icon不属于图片组件，只是效果类似；
- 建议优先使用Icon；
  - 体积更小；
  - 不会失真和模糊；
  - 多个图标可以存放在一个文件中，方便管理；
  - 全平台通用。
- Image组件可以用于网络、项目中的资源图片，设备中存储的图片等

## 属性

- 加载网络图片

  ```dart
  Image.network(
        "http://pic1.win4000.com/pic/c/cf/cdc983699c.jpg",
  ),
  ```

- 加载项目中的资源图片

  - 先将图片copy到assets/images/目录瞎，需要手动创建这个目录
  - 在pubspec.yaml中配置该文件

  ```dart
  assets:
     - assets/images/
  ```

  - 加载图片

    ```dart
    Image.asset(
         'assets/images/rb.jpg',
    ),
    ```

- 加载设备上的图片：略

- 图片可以直接设置width和height

  ```dart
  width: 50,
  height: 50,
  ```

- 图片的填充模式

  ```dart
  fit: BoxFit.cover,
  ```

  - fill：完全填充，宽高比可能会变;
  - contain：等比拉伸，直到一边填充满;
  - cover：等比拉伸，直到2边都填充满，此时一边可能超出范围;
  - fitWidth：等比拉伸，宽填充满;
  - fitHeight：等比拉伸，高填充满;
  - none：当组件比图片小时，不拉伸，超出范围截取;
  - scaleDown：当组件比图片小时，图片等比缩小，效果和**contain**一样;

- color和colorBlendMode颜色和图片进行颜色混合：略

- repeat表示Image有空余时，重复显示图片

  ```dart
  repeat: ImageRepeat.repeatX,
  ```

  

