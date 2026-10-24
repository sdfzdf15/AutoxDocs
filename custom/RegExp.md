---
created: 2026-10-24T08:35:56 (UTC +08:00)
tags: [JavaScript RegExp 对象]
source: https://www.runoob.com/jsref/jsref-obj-regexp.html
author: 
---

# JavaScript RegExp 对象

> ## Excerpt
> JavaScript RegExp 对象  RegExp 对象 正则表达式是描述字符模式的对象。 正则表达式用于对字符串模式匹配及检索替换，是对字符串执行模式匹配的强大工具。  语法   var patt=new RegExp(pattern,modifiers); 或者更简单的方式: var patt=/pattern/modifiers;   pattern（模式） 描述了表达式的模式 	modifiers(修饰符) 用于指定全局匹..

---
___

## RegExp 对象

正则表达式是描述字符模式的对象。

正则表达式用于对字符串模式匹配及检索替换，是对字符串执行模式匹配的强大工具。

## 语法

var patt=new RegExp(pattern,modifiers);

或者更简单的方式:

var patt=/pattern/modifiers;

-   pattern（模式） 描述了表达式的模式
-   modifiers(修饰符) 用于指定全局匹配、区分大小写的匹配和多行匹配

> **注意：**当使用构造函数创造正则对象时，需要常规的字符转义规则（在前面加反斜杠 \\）。比如，以下是等价的：
> 
> ```
> var re = new RegExp("\\w+");
> var re = /\w+/;
> ```

更多关于 RegExp 对象请阅读我们的 [JavaScript RegExp 对象教程](https://www.runoob.com/js/js-obj-regexp.html)。

___

## 修饰符

修饰符用于执行区分大小写和全局匹配:

|修饰符|描述|
|---|---|
|[i](https://www.runoob.com/js/jsref-regexp-i.html)|<small>执行对大小写不敏感的匹配。</small>|
|[g](https://www.runoob.com/js/jsref-regexp-g.html)|<small>执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。</small>|
|m|<small>执行多行匹配。</small>|

## 方括号

方括号用于查找某个范围内的字符：

|表达式|描述|
|---|---|
|[<code>[abc]</code>](https://www.runoob.com/jsref/jsref-regexp-charset.html)|<small>查找方括号之间的任何字符。</small>|
|[<code>[^abc]</code>](https://www.runoob.com/jsref/jsref-regexp-charset-not.html)|<small>查找任何不在方括号之间的字符。</small>|
|<code>[0-9]</code>|<small>查找任何从 0 至 9 的数字。</small>|
|<code>[a-z]</code>|<small>查找任何从小写 a 到小写 z 的字符。</small>|
|<code>[A-Z]</code>|<small>查找任何从大写 A 到大写 Z 的字符。</small>|
|<code>[A-z]</code>|<small>查找任何从大写 A 到小写 z 的字符。</small>|
|<code>[adgk]</code>|<small>查找给定集合内的任何字符。</small>|
|<code>[^adgk]</code>|<small>查找给定集合外的任何字符。</small>|
|<code>(red\|blue\|green)</code>|<small>查找任何指定的选项。</small>|

## 元字符

元字符（Metacharacter）是拥有特殊含义的字符：

|元字符|描述|
|---|---|
|[<code>.</code>](https://www.runoob.com/jsref/jsref-regexp-dot.html)|<small>查找单个字符，除了换行和行结束符。</small>|
|[<code>\w</code>](https://www.runoob.com/jsref/jsref-regexp-wordchar.html)|<small>查找数字、字母及下划线。</small>|
|[<code>\W</code>](https://www.runoob.com/jsref/jsref-regexp-wordchar-non.html)|<small>查找非单词字符。</small>|
|[<code>\d</code>](https://www.runoob.com/jsref/jsref-regexp-digit.html)|<small>查找数字。</small>|
|[<code>\D</code>](https://www.runoob.com/jsref/jsref-regexp-digit-non.html)|<small>查找非数字字符。</small>|
|[<code>\s</code>](https://www.runoob.com/jsref/jsref-regexp-whitespace.html)|<small>查找空白字符。</small>|
|[<code>\S</code>](https://www.runoob.com/jsref/jsref-regexp-whitespace-non.html)|<small>查找非空白字符。</small>|
|[<code>\b</code>](https://www.runoob.com/jsref/jsref-regexp-begin.html)|<small>匹配单词边界。</small>|
|[<code>\B</code>](https://www.runoob.com/jsref/jsref-regexp-begin-not.html)|<small>匹配非单词边界。</small>|
|<code>\0</code>|<small>查找 NULL 字符。</small>|
|[<code>\n</code>](https://www.runoob.com/jsref/jsref-regexp-newline.html)|<small>查找换行符。</small>|
|<code>\f</code>|<small>查找换页符。</small>|
|<code>\r</code>|<small>查找回车符。</small>|
|<code>\t</code>|<small>查找制表符。</small>|
|<code>\v</code>|<small>查找垂直制表符。</small>|
|[<code>\xxx</code>](https://www.runoob.com/jsref/jsref-regexp-octal.html)|<small>查找以八进制数 xxx 规定的字符。</small>|
|[<code>\xdd</code>](https://www.runoob.com/jsref/jsref-regexp-hex.html)|<small>查找以十六进制数 dd 规定的字符。</small>|
|[<code>\uxxxx</code>](https://www.runoob.com/jsref/jsref-regexp-unicode-hex.html)|<small>查找以十六进制数 xxxx 规定的 Unicode 字符。</small>|
|<code>\p{Property}</code>|<small>ES2018+ Unicode 属性转义，需配合 /u 标志。例如 <code>\p{Letter}</code> 匹配任意语言的字母。</small>|
|<code>\P{Property}</code>|<small>ES2018+ 匹配不满足指定 Unicode 属性的字符，<code>\p{...}</code> 的取反。</small>|
|<code>\k&lt;name&gt;</code>|<small>命名捕获组的反向引用，作用同 <code>\1</code> 但用名字代替编号。需配合 <code>(?&lt;name&gt;...)</code> 使用。<br>例如，<code>/(?&lt;word&gt;\w+) \k&lt;word&gt;/</code> 匹配重复出现的同一个单词。<br>拆开看就两步：<br>1. (?<word>\w+) — 捕捉一个单词，取名为 word<br>2.\k<word> — 在正则后面再引用一次这个名字，要求一模一样<br>本质上 \k<name> 和 \1 是同一个东西，只是一个用名字找人、一个用编号找人。名字显然比「第 3 个括号」好记。</small>|
|<code>\1</code>, <code>\2</code>, ...|<small>反向引用，匹配第 n 个捕获分组的内容。<code>/(\w+)\s+\1/</code> 匹配重复单词。</small>|

## 量词

|量词|描述|
|---|---|
|[+](https://www.runoob.com/jsref/jsref-regexp-onemore.html)|<small><code>n+</code> n 代表任意字符或模式。<br>前面的内容出现 1 次或多次（至少 1 次）。<br>例如，<code>/a+/</code> 匹配 "caandy" 中的 "aa"。</small>|
|[*](https://www.runoob.com/jsref/jsref-regexp-zeromore.html)|<small><code>n*</code> n 代表任意字符或模式。<br>前面的内容出现 0 次或多次（可有可无，也可连续出现）。<br>例如，<code>/bo*/</code> 匹配 "boooo" 或单独的 "b"。</small>|
|[?](https://www.runoob.com/jsref/jsref-regexp-zeroone.html)|<small><code>n?</code> n 代表任意字符或模式。<br>前面的内容出现 0 次或 1 次（可有可无）。<br>例如，<code>/colou?r/</code> 同时匹配 "color" 和 "colour"。</small>|
|[{X}](https://www.runoob.com/jsref/jsref-regexp-nx.html)|<small><code>n{X}</code> n 代表任意字符或模式。<br>前面的内容恰好出现 X 次。<br>例如，<code>/\d{3}/</code> 匹配恰好 3 位数字（如 "123"）。</small>|
|[{X,}](https://www.runoob.com/jsref/jsref-regexp-nxcomma.html)|<small><code>n{X,}</code> n 代表任意字符或模式。<br>前面的内容至少出现 X 次（X 次或更多）。<br>例如，<code>/\d{2,}/</code> 匹配 2 位及以上的数字。</small>|
|[{X,Y}](https://www.runoob.com/jsref/jsref-regexp-nxy.html)|<small><code>n{X,Y}</code> n 代表任意字符或模式。<br>前面的内容出现 X 到 Y 次（最少 X 次，最多 Y 次）。<br>例如，<code>/\d{2,4}/</code> 匹配 2~4 位数字。</small>|
|[$](https://www.runoob.com/jsref/jsref-regexp-ndollar.html)|<small><code>n$</code> n 代表任意字符或模式。<br>匹配以 n 结尾的字符串。<br>例如，<code>/\.js$/</code> 匹配以 ".js" 结尾的文件名。</small>|
|[^n](https://www.runoob.com/jsref/jsref-regexp-ncaret.html)|<small><code>^n</code> n 代表任意字符或模式。<br>匹配以 n 开头的字符串。<br>例如，<code>/^http/</code> 匹配以 "http" 开头的网址。</small>|
|[?=](https://www.runoob.com/jsref/jsref-regexp-nfollow.html)|<small><code>?=n</code> n 代表任意字符或模式。<br>后面必须紧跟着 n（正向先行断言，不消耗字符）。<br>例如，<code>/\d+(?=元)/</code> 匹配 "100元" 中的 "100"。</small>|
|[?!](https://www.runoob.com/jsref/jsref-regexp-nfollow-not.html)|<small><code>?!n</code> n 代表任意字符或模式。<br>后面不能紧跟着 n（负向先行断言，不消耗字符）。<br>例如，<code>/\d+(?!元)/</code> 不匹配 "100元" 但匹配 "100 美元" 中的 "100"。</small>|
|<code>(?&lt;=y)</code>|<small><code>(?&lt;=y)</code> y 代表任意字符或模式。<br>前面必须是 y（后行断言，ES2018+，不消耗字符）。<br>例如，<code>/(?&lt;=\$)\d+/</code> 匹配 "$100" 中的 "100"。</small>|
|<code>(?&lt;!y)</code>|<small><code>(?&lt;!y)</code> y 代表任意字符或模式。<br>前面不能是 y（后行否定断言，ES2018+，不消耗字符）。<br>例如，<code>/(?&lt;!\$)\d+/</code> 匹配 "€100" 中的 "100"，但不匹配 "$100"。</small>|
|<code>*?</code>|<small><code>n*?</code> n 代表任意字符或模式。<br>（非贪婪）前面的内容出现 0 次或多次，尽量少匹配。<br>例如，<code>/&lt;.*?&gt;/</code> 匹配 "&lt;a&gt;&lt;b&gt;" 时只取 "&lt;a&gt;"，而不是整串。</small>|
|<code>+?</code>|<small><code>n+?</code> n 代表任意字符或模式。<br>（非贪婪）前面的内容出现 1 次或多次，尽量少匹配。<br>例如，<code>/a+?/</code> 匹配 "aaa" 时只取 "a"。</small>|
|<code>??</code>|<small><code>n??</code> n 代表任意字符或模式。<br>（非贪婪）前面的内容出现 0 次或 1 次，优先选 0 次。</small>|
|<code>{X,}?</code>|<small><code>n{X,}?</code> n 代表任意字符或模式。<br>（非贪婪）前面的内容至少出现 X 次，尽量少匹配。</small>|
|<code>{X,Y}?</code>|<small><code>n{X,Y}?</code> n 代表任意字符或模式。<br>（非贪婪）前面的内容出现 X 到 Y 次，尽量少匹配。</small>|

## 分组与捕获

|表达式|描述|
|---|---|
|`( )`|<small><code>(abc)</code> abc 代表任意字符或模式。<br>捕获分组：匹配内容会被记住，之后可通过 <code>\1</code>、<code>\2</code> 或 <code>$1</code> 引用。<br>例如，<code>/(\w+)\s+\1/</code> 匹配重复出现的单词。<br>例如，<code>(\d+)</code> 匹配任意数字。</small>|
|<code>(?: )</code>|<small><code>(?:abc)</code> abc 代表任意字符或模式。<br>非捕获分组：只分组不记住，适合给量词划定范围但不需要引用结果。<br>例如，<code>/(?:https?:\/\/)?/</code> 把协议部分作为可选整体。,<br>只是把这部分括起来方便加符号，但不提取内容。</small>|
|<code>(?&lt;name&gt;)</code>|<small><code>(?&lt;name&gt;abc)</code> name 是给这段匹配取的名字，abc 才是真正要匹配的内容。<br>命名捕获分组（ES2018+）：给匹配内容起个名字，比数字编号更直观。<br>用法：正则中用 <code>\k&lt;name&gt;</code> 再次引用同名捕获组（作用同 <code>\1</code>，但用名字代替编号），结果中通过 <code>result.groups.name</code> 取值。<br>例如，<code>/(?&lt;year&gt;\d{4})-(?&lt;month&gt;\d{2})/</code> 匹配 "2024-10" 后 <code>result.groups.year</code> 得 "2024"。<br>又如，<code>/(?&lt;word&gt;\w+) \k&lt;word&gt;/</code> 匹配重复单词，比 <code>/(\w+) \1/</code> 更易读。</small>|

## RegExp 对象方法

|方法|描述|
|---|---|
|[compile](https://www.runoob.com/jsref/jsref-regexp-compile.html)|<small>在 1.5 版本中已废弃。 编译正则表达式。</small>|
|[exec](https://www.runoob.com/jsref/jsref-exec-regexp.html)|<small>检索字符串中指定的值。返回找到的值，并确定其位置。</small>|
|[test](https://www.runoob.com/jsref/jsref-test-regexp.html)|<small>检索字符串中指定的值。返回 true 或 false。</small>|
|[toString](https://www.runoob.com/jsref/jsref-regexp-tostring.html)|<small>返回正则表达式的字符串。</small>|

## 支持正则表达式的 String 对象的方法

|方法|描述|FF|IE|
|---|---|---|---|
|[search](https://www.runoob.com/js/jsref-search.html)|<small>检索与正则表达式相匹配的值。</small>|1|4|
|[match](https://www.runoob.com/js/jsref-match.html)|<small>找到一个或多个正则表达式的匹配。</small>|1|4|
|[replace](https://www.runoob.com/js/jsref-replace.html)|<small>替换与正则表达式匹配的子串。</small>|1|4|
|[split](https://www.runoob.com/js/jsref-split.html)|<small>把字符串分割为字符串数组。</small>|1|4|

___

## RegExp 对象属性

|属性|描述|
|---|---|
|[constructor](https://www.runoob.com/jsref/jsref-regexp-constructor.html)|<small>返回一个函数，该函数是一个创建 RegExp 对象的原型。</small>|
|[global](https://www.runoob.com/jsref/jsref-regexp-global.html)|<small>判断是否设置了 "g" 修饰符</small>|
|[ignoreCase](https://www.runoob.com/jsref/jsref-regexp-ignorecase.html)|<small>判断是否设置了 "i" 修饰符</small>|
|[lastIndex](https://www.runoob.com/jsref/jsref-lastindex-regexp.html)|<small>用于规定下次匹配的起始位置</small>|
|[multiline](https://www.runoob.com/jsref/jsref-multiline-regexp.html)|<small>判断是否设置了 "m" 修饰符</small>|
|[source](https://www.runoob.com/jsref/jsref-source-regexp.html)|<small>返回正则表达式的匹配模式</small>|