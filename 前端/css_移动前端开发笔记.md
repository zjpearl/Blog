# 移动前端开发

知识点一:获取html对象，根据获得对象的字符串显示对应图片

es6中允许使用 `` 创建字符串模板,可以直接写回车空格编写html或文本

在字符串拼接中，可以代替 ' ' 以及  " " ，在 ` ` 中可以使用 ${ }直接把变量与字符串拼接起来



```html
<span style="display:none;">1</span>
```

```javascript
index:tag.querySelector('span').innerHTML

```

```javascript
 $api.dom('.empty').innerHTML = `<img src="../image/${api.pageParam.index}.jpg" height="180" width="300" />`;

```

```
/ 1. 编译模板函数
var tempFn = doT.template("<h1>Here is a sample template {{=it.foo}}</h1>");
// 2. 渲染模板函数
var resultText = tempFn({foo: 'with doT'});
```

doT.template方法返回值为function类型：tempFn，tempFn接收的参数是**模板渲染时可传入的数据**（与前面编译时的数据不是同一份数据）。