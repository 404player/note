## HTML

### html基本格式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>这是一个标题</h1>
    <p>这是一个段落</p>
</body>
</html>
```

### html的注释

```html
<!--解释的下面一条或多条语句-->
```

### html代码规范

- HTML 标记以区分大小写，建议 小写
- HTML 标记属性可有可无，有的标记是没有属性的
- 单标签没有内容

### html标记分类

- 单标记： 标记只有一个，不是修饰内容的而是显示某一个内容的，如图片，设置编码，设置关键词等  

```html
<标记名称 属性="值" 属性2="值2" />
```

```html
<img src="图片在服务器中的路径" />
```

- 双标记： 是修饰内容的标记，有开始有结束标记，中间要写修饰的内容

```html
<标记名称 属性="值">要修饰的内容</标记名称>
```

```html
<div>
    content
</div>
<font color="red">
    text
</font>
```

### body的属性

- `Bgcolor`：背景颜色

```html
<body bgcolor="color">
    
</body>
```

颜色： 单词、16进制、 rgb方式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<!--<body bgcolor="">-->
<!--<body bgcolor="#ff00ff"> 16进制，由0-f的字符组合，组合成6个字符-->
<body bgcolor="rgb(0,0,0)"> <!--三原色，每个颜色都是从0-255-->
</body>
</html>
```

- `Background`：背景图片

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<!--如果没有屏幕那么大，就会平铺-->
<body background="./img/pic1.png">
</body>
</html>
```

### 字体修饰标记

- `<font></font>`：文本的修饰

```html
<font>文本的内容</font>
```

```html
<!--Font的标记属性：-->
Color: 文本的颜色  值： 颜色
Size： 文本的大小  值：1-6
```

- `<i></i>`:  斜体
- `<b></b>`：加粗
- `<u></u>`： 下划线
- `<s></s>`： 删除线
- `<sub></sub>`: 下标
- `<sup></sup>`：上标

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <font color="red" size="5"><i>泰牛PHP07</i></font>
    <i>你好啊，我是斜体</i>
    <b>你好啊，我是粗体</b>
    <u>你好啊，我是下划线</u>
    <s>你好啊，我是删除线</s>
    <font>今天3<sup>o</sup>C</font>
    <font>H<sub>2</sub>O</font>
</body>
</html>
```

