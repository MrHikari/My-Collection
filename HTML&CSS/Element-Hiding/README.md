#### 元素隐藏的不同方式

**dispaly**, **visibility**, **opacity**, **height**&**border**。

##### 简单示例

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>title</title>
    <style>
      div {
        width: 300px;
        height: 200px;
        border: 1px solid red;
      }
    </style>
  </head>
  <body>
    <input type="button" value="显示效果" id="btn" />
    <div id="dv">示例</div>
    <script src="common.js"></script>
    <script>
      document.getElementById("btn").onclick = function() {
        //隐藏div
        //不占位
        //my$("dv").style.display="none";
        //占位
        //my$("dv").style.visibility="hidden";
        //占位
        //my$("dv").style.opacity=0;
        //占位
        //my$("dv").style.height="0px";
        //my$("dv").style.border="0px solid red";
      };
    </script>
  </body>
</html>
```
