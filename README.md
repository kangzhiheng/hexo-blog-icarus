## Blog

网站：www.kangzhiheng.top

## 需求：实现markdown里的时间线

### 相关源码

1. markdown嵌入HTML源码：\source\yulu\index.md

   ```html
   <div class="time-axis-main">
   	<ul class="time-axis"></ul>
   </div>
   <script src="/js/yulu.js"></script>
   <br>
   <br>
   ```

   - 其中包含一个类`time-axis-main`，定义在\themes\hexo-theme-amazing\source\css\base.styl里；
   - 还包含了一个js文件`yulu.js`，路径为\themes\hexo-theme-amazing\source\js\yulu.js；

2. `time-axis-main`定义：\themes\hexo-theme-amazing\source\css\base.styl里；

   ```stylus
   .time-axis-main {
       width: 90%;
       margin: 30px auto;
       .time-axis {
           margin: 0;
           padding: 0;
           position: relative;
       }
   
       .time-axis:before {
           content: '';
           position: absolute;
           left: 93px;
           top: 15px;
           width: 1px;
           height: 100%;
           background-color: #E4E4E4;
       }
   
       .time-axis-item {
           list-style: none;
           padding-left: 150px;
           position: relative;
           line-height: 45px;
           font-size: 14px;
           color: #141414;
       }
   
       .time-axis-date {
           position: absolute;
           left: -1.9%;
           top: 0;
           color: #3273dc;
           font-style: italic;
           font-weight: bold;
       }
   
       .time-axis-title {
           background-color: whitesmoke;
           border-left: 3px solid #6190e8;
           padding: 0.8em 1.0em;
           font-weight: bold;
           line-height: 25px;
       }
   
       .time-axis-achievement {
           background-color: whitesmoke;
           border-left: 3px solid #e89b44;
           padding: 0em 1.0em;
           font-style: italic;
           margin-bottom: 30px;
       }
   
       .time-axis-date span {
           position: absolute;
           right: -25px;
           top: 35%;
           display: block;
           width: 13px;
           height: 13px;
           border: 1px solid #ccc;
           border-radius: 100%;
           background-color: #fff;
       }
   
       .time-axis-date span:after {
           content: '';
           position: absolute;
           left: 0;
           top: 0;
           right: 0;
           bottom: 0;
           width: 7px;
           height: 7px;
           margin: auto;
           background-color: #ccc;
           border: 1px solid #ccc;
           border-radius: 100%;
       }
   
       .time-axis-item:first-child .time-axis-date span {
           border-color: #48BEB2;
       }
   
       .time-axis-item:first-child .time-axis-date span:after {
           background-color: #48BEB2;
           border-color: #48BEB2;
       }
   }
   ```

3. js文件`yulu.js`：\themes\hexo-theme-amazing\source\js\yulu.js；

   ```javascript
   $(function () { // 获取记录数据
       $.getJSON("../json_data/yulu.json", function (data) {
           $.each(data, function (i, e) {
               var html = '<li class="time-axis-item">' +
                   '<div class="time-axis-date">' + e.date + '<span></span></div>' +
                   '<div class="time-axis-title">' + e.title + '</div>' +
                   '<p class="time-axis-achievement">' + e.achievement + '</p>' +
                   '</li>';
               $('.time-axis').append(html);
           });
       })
   });
   ```

4. `yulu.js`读取的文件格式为`json`，路径为：\themes\hexo-theme-amazing\source\json_data\yulu.json，里面的内容类似为：

   ```json
   [
       {
           "date": "2020.02.10",
           "title": "星星为什么看起来如此渺小，是因为它把自己放得太高。",
           "achievement": "《秦时明月》"
       },
       {
           "date": "2020.02.10",
           "title": "如果提出的问题本身就有问题，那么答案又有什么用呢？",
           "achievement": "《秦时明月》"
       }
   ]
   ```

我顺藤摸瓜，找到了这些源码，最后实现的效果[展示](https://www.kangzhiheng.top/yulu/)

![时间轴](https://pic.downk.cc/item/5e9fe096c2a9a83be556789d.png)

### 我的问题：

- 怎么单独把这几个文件组合在一起，有上图的效果？

