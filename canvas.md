# Canvas

canvas提供了使用画布进行2D画图的支持，可用于简单的小游戏开发或者图片编辑。使用canvas可以轻松地在一张图片或一个界面上绘制各种线与图形。

canvas的坐标系为平面直角坐标系，以屏幕左上角为原点，屏幕上边为x轴正方向，屏幕左边为y轴正方向。例如分辨率为1920*1080的屏幕上，画一条从屏幕左上角到屏幕右下角的线段为:

```js
canvas.drawLine(0, 0, 1080, 1920, paint);
```

canvas的绘制依赖于画笔Paint, 通过设置画笔的粗细、颜色、填充等可以改变绘制出来的图形。例如绘制一个红色实心正方形为：
```js
var paint = new Paint();
//设置画笔为填充，则绘制出来的图形都是实心的
paint.setStyle(Paint.STYLE.FILL);
//设置画笔颜色为红色
paint.setColor(colors.RED);
//绘制一个从坐标(0, 0)到坐标(100, 100)的正方形
canvas.drawRect(0, 0, 100, 100, paint);
```

如果要绘制正方形的边框，则通过设置画笔的Style来实现：
```js
var paint = new Paint();
//设置画笔为描边，则绘制出来的图形都是轮廓
paint.setStyle(Paint.STYLE.STROKE);
//设置画笔颜色为红色
paint.setColor(colors.RED);
//绘制一个从坐标(0, 0)到坐标(100, 100)的正方形
canvas.drawRect(0, 0, 100, 100, paint);
```

结合画笔canvas可以绘制基本图形、图片等。

## canvas.drawARGB(a, r, g, b)

## canvas.draw
 
 例子：
 ```js
 let flo = floaty.rawWindow(
    <frame>
        <frame id="frame">
            <canvas id="board" w="{{device.width}}" h="{{device.height}}" />
        </frame>
    </frame>
);
flo.setTouchable(false);
//画笔
let paint = new Paint();
flo.board.on("draw", function(canvas) {
    //清空内容，以便后面重绘
    canvas.drawColor(0xFFFFFF, android.graphics.PorterDuff.Mode.CLEAR)
    //设置画笔宽度
    paint.setStrokeWidth(10)
    // 设置画笔颜色为红色(paint.setColor()这个函数部分手机显示有问题,用这个paint.setARGB())
    paint.setARGB(255, 255, 0, 0)
    paint.setStyle(Paint.Style.STROKE); // 填充 FILL实心的 STROKE空心

    canvas.drawRect(200, 300, 400, 500, paint)

});

setInterval(function() {}, 1000);
 ```