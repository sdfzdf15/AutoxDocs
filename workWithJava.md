# WorkWithJava

关于调用 java [请查看这里](https://p-bakker.github.io/rhino/tutorials/scripting_java/) ，一个由 Rhino 维护者使用 GitHub Pages 部署的 Rhino 文档网站


[官方 issues 关于原 Rhino 文档地址404 的讨论](https://github.com/mozilla/rhino/issues/954#issuecomment-949763810)

# 和Java交互 

> ## Excerpt
>
> Rhino提供了非常方便地和Java交互的能力。 liveConnect：与JavaScript的Java通信 Rhino允许您从JavaScript中创建Java类并调用Java方法。例如: 访问JavaBean属性 Java类可以使用getter和Setter方法定义JavaBean属性。例如，以下类定义了两个属性： 定义的两个属性是 age_和_...

___

Rhino提供了非常方便地和Java交互的能力。

## liveConnect：与JavaScript的Java通信

Rhino允许您从JavaScript中创建Java类并调用Java方法。例如:

```javascript
let builder = new java.lang.Builder();
builder.append('test');
builder.append(1);
console.log(builder.toString());
```

## 访问JavaBean属性

Java类可以使用getter和Setter方法定义JavaBean属性。例如，以下类定义了两个属性：

```java
public class Me {  
 public int getAge() \{ return mAge; \}  
 public void setAge(int anAge) \{ mAge = anAge; \}  
 public String getSex() \{ return "male"; \}  
 private int mAge;  
};
```

定义的两个属性是 _age\_和\_sex_。 \_sex\_属性是只读的：它没有Setter。

使用Rhino我们可以访问Bean属性，就像它们一样的JavaScript属性。我们也可以继续调用定义属性的方法。

```javascript
let me = new Me();
console.log(me.sex);
me.age = 33;
console.log(me.age);
console.log(me.getAge());
```

由于\_sex\_属性是只读的，因此我们不允许写入它。

## 导入Java类和包

上面我们看到了importPackage函数的使用来从特定的Java包导入所有类。还有importClass，它导入单个类。

你可以直接使用`android.view.View`来表示Android中的View类，默认支持的顶级包名前缀为`com`, `android`, `java`, `org`，对于其他包名，需要使用`Packages`对象，比如`Packages.javax.xml.xpath.XPath`或`Packages["javax.xml.xpath.XPath"]`。

也可以使用`importClass`或`importPackage`函数来导入Java/Android中的包名或类，比如：

```javascript
importClass("android.view.KeyEvent");
// 或者
let KeyEvent = android.view.KeyEvent;
// 或者
importPackage("android.view");
```

## 扩展:Java类并使用JavaScript实现Java接口

例如为某个UI中的控件设置点击监听OnClickListener:

```javascript
"ui";
$ui.layout(
  <frame>
    <button id="btn" text="BUTTON"/>
  </frame>
);

let listener = new android.view.View.OnClickListener(function(view) {
  console.log("clicked");
});
$ui.btn.setOnClickListener(listener);
```

当我们键入`new android.view.View.OnClickListener`时，rhino实际上创建了一个新的Java类，它实现了OnClickListener并将从该类转发给JavaScript对象的调用。

Rhino也允许将JavaScript函数直接传递给Java方法，如果相应的参数是Java接口，它具有单个方法或其所有方法具有相同数量的参数，相应的参数具有相同类型的参数。例如：

```javascript
$ui.btn.setOnClickListener(function(view) {
  console.log("clicked");
});
```

若Java接口有多个方法，则可以传入一个JavaScript对象来实现他，比如对于Java接口：

```java
/**
* Interface definition for a callback to be invoked when this view is attached
* or detached from its window.
*/
public interface OnAttachStateChangeListener {
        /**
        * Called when the view is attached to a window.
        * @param v The view that was attached
        */
        public void onViewAttachedToWindow(View v);
        /**
        * Called when the view is detached from a window.
        * @param v The view that was detached
        */
        public void onViewDetachedFromWindow(View v);
}
```

我们可以在JavaScript中这样实现：

```javascript
let listener = new android.view.View.OnAttachStateChangeListener({
  onViewAttachedToWindow: function(view) {
    console.log('attached');
  },
  onViewDetachedFromWindow: function(view) {
    console.log('detached');
  }
});
$ui.btn.addOnAttachStateChangeListener(listener);
```

## JavaAdapter构造函数(推荐)

使用JavaAdapter也可以用于实现接口，同时可以用于继承普通类或抽象类。他的原理是动态生成一个类。

```javascript
let listener = new JavaAdapter(android.view.View.OnAttachStateChangeListener, {
  onViewAttachedToWindow: function(view) {
    console.log('attached');
  },
  onViewDetachedFromWindow: function(view) {
    console.log('detached');
  }
});
```

如果我们想实现多个接口，则：

```javascript
let listener = new JavaAdapter(android.view.View.OnAttachStateChangeListener, java.lang.Runnable, {
  onViewAttachedToWindow: function(view) {
    console.log('attached');
  },
  onViewDetachedFromWindow: function(view) {
    console.log('detached');
  },
  run: function() {
    console.log('run');
  }
});
```

一般来说，语法是:

```javascript
new JavaAdapter(java-class, [java-class，...]，javascript-object, [args...])
```

最多一个java-class是java类，剩下的java-class参数是接口。结果将是继承指定的Java类并实现所有的Java接口，并将任何调用转发给javascript-object的方法。args参数用于指定构造Java类的构造函数。

比如继承View，重写onDraw函数:

```javascript
"ui";

$ui.layout(
  <vertical>
    <frame id="container"/>
  </vertical>
);

let paint = new Paint();
let view = new JavaAdapter(android.view.View, {
  onDraw: function (canvas) {
    // 调用父类View的onDraw
    this.super$onDraw(canvas);      
    canvas.drawRect(500, 500, 1000, 1000, paint);
  },
  // activity为android.view.View的构造函数参数
}, activity);

$ui.container.addView(view);
```