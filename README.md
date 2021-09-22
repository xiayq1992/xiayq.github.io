# xiayq.github.io
Android Develop
前言
Flutter的布局在此前的文章多多少少用过，但是没有具体讲过，Flutter的布局实际上也由Widget来控制的，在Flutter官网上并没有对布局Widget进行分类，这里将布局Widget根据子元素排列方式分为以下几种：

线性布局Widget
流式布局Widget
层式布局Widget
弹性布局Widget
下面分别介绍这几种布局Widget。

1.线性布局Widget
线性布局类似于Android中的LinearLayout，可以垂直也可以水平排列，Flutter的线性布局有两个：

Row：水平方向的线性布局。
Column：垂直方向的线性布局。
Row和Column的使用方法类似，举个Row的例子，Column自然而然也就会了。

JAVA
import 'package:flutter/material.dart';

void main() => runApp(RowWidget());

class RowWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: "Flutter",
      home: Scaffold(
        appBar: AppBar(
          title: Text("Row示例"),
        ),
        body: Padding(
          padding: EdgeInsets.all(40.0),
          child: Row(
            children: <Widget>[
              Text("Android进阶之光"),
              Text("Android进阶解密"),
            ],
          ),
        ),
      ),
    );
  }
}
效果如下图所示。
ZKvSQP.png

2.流式布局Widget
流式布局就是把超出屏幕显示范围的控件自动换行，在Android中并没有现成的流式布局控件，都是开发人员自己封装的，比如鸿洋封装的FlowLayout。Flutter中提供了Wrap来实现流式布局。

JAVA
import 'package:flutter/material.dart';

void main() => runApp(ChipWidget());

class ChipWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: "Flutter",
      home: Scaffold(
        appBar: AppBar(
          title: Text("流式布局示例"),
        ),
        body: Padding(
          padding: EdgeInsets.all(20.0),
          child: Wrap(
            direction: Axis.horizontal, //主轴的方向
            spacing: 8.0, // 主轴方向的间距
            runSpacing: 12.0, // 交叉轴方向的间距
            children: <Widget>[
              Chip(//1
                avatar: CircleAvatar(
                    backgroundColor: Colors.blue, child: Text('1')),
                label: Text('Android进阶之光'),
              ),
              Chip(
                avatar: CircleAvatar(
                    backgroundColor: Colors.blue, child: Text('2')),
                label: Text('Android进阶解密'),
              ),
              Chip(
                avatar: CircleAvatar(
                    backgroundColor: Colors.blue, child: Text('3')),
                label: Text('Android进阶？'),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

注释1处的Chip意为碎片，实际上是一个标签Widget，这个例子中定义了三个标签来体现流式布局。
Wrap有很多属性，它的构造函数如下：

JAVA
Wrap({
  Key key,
  this.direction = Axis.horizontal, //1
  this.alignment = WrapAlignment.start,//2
  this.spacing = 0.0,
  this.runAlignment = WrapAlignment.start,
  this.runSpacing = 0.0,
  this.crossAxisAlignment = WrapCrossAlignment.start,
  this.textDirection,
  this.verticalDirection = VerticalDirection.down,
  List<Widget> children = const <Widget>[],
}) : super(key: key, children: children);

注释1处的direction属性，它用于设定主轴的方向（子Widget排列的方向），默认值为Axis.horizontal，部分属性见下表：

参数名称	参数类型	含义
direction	Axis	主轴的方向
alignment	WrapAlignment	子Widget 在主轴上的对齐方式
runAlignment	WrapAlignment	每行或每列的对齐方式
runSpacing	double	每行或每列之间的间距
crossAxisAlignment	WrapCrossAlignment	子Widget在交叉轴上的对齐方式
textDirection	TextDirection	子Widget在主轴方向上的布局顺序
verticalDirection	VerticalDirection	子Widget在交叉轴方向上的布局顺序
运行后的效果如下图所示。
ZlZfWq.png

3.层式布局Widget
层式布局类似于Android的FrameLayout，在Flutter可以通过Stack来实现层式布局，其子Widget 会按照添加顺序确定显示层级，后面添加的会覆盖在前面添加的Widget上面。
为了确定子Widget到父容器四个角的位置，Stack将子Widget分为两类：
