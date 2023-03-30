# ECharts画图学习

配置项文档：https://echarts.apache.org/zh/option.html#title

## 1. 柱形图【bar】
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>柱形图入门</title>
    <!-- 1. 引入 echarts.js 文件-->
    <script src="lib/echarts.js"></script>

</head>
<body>
    <!-- 2. 准备一个呈现图表的盒子 div -->
    <div style="width: 600px;height: 400px"></div>

    <script>
        // 3. 初始化 echarts 实例对象
        // 参数，dom，决定图表最终呈现的位置
        var mCharts = echarts.init(document.querySelector('div'));
        <!-- 4. 准备配置项 -->
        var option = {
            xAxis: {
                type: 'category',
                data: ['小明', '小红', '小王']
            },
            yAxis: {
                type: 'value'
            },
            series: [
                {
                    name: '语文',
                    type: 'bar', // line：折线图
                    data:[70, 92, 87],
                    itemStyle:{
                        color: 'red'
                    }
                }
            ]
        }
        <!-- 5. 将配置项设置给 echarts 实例对象 -->
        mCharts.setOption(option)
    </script>
</body>
</html>
```
![](https://img2023.cnblogs.com/blog/2171496/202303/2171496-20230316104544977-1921306207.png)
注意: 坐标轴xAxis 或 者yAxis 中的配置, type 的值主要有两种: category 和 value
- 如果type属性的值为category ,那么需要配置data 数据, 代表在x 轴的呈现. 
- 如果type 属性配置为value , 那么无需配置data , 此时 y 轴会自动去series 下找数据进行图表的绘制
### 1.1 柱形图常见效果
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>柱形图入门</title>
    <!-- 1. 引入 echarts.js 文件-->
    <script src="lib/echarts.js"></script>

</head>
<body>
<!-- 2. 准备一个呈现图表的盒子 div -->
<div style="width: 600px;height: 400px"></div>

<script>
    // 3. 初始化 echarts 实例对象
    // 参数，dom，决定图表最终呈现的位置
    var mCharts = echarts.init(document.querySelector('div'));

    //  准备数据
    var xDataArr = ['张三', '李四', '王五', '闰土', '小明', '茅台', '二妞', '大强']
    var yDataArr = [88, 92, 63, 77, 94, 80, 72, 86]


    <!-- 4. 准备配置项 -->
    var option = {
        xAxis: {
            type: 'category',
            data: xDataArr
        },
        yAxis: {
            type: 'value'
        },
        series: [
            {
                name: '语文',
                type: 'bar', // line：折线图
                data: yDataArr,
                itemStyle:{
                    color: 'red'
                },
                markPoint:{ //  极值
                    data:[
                        {
                            type: 'max', name: '最大值'
                        },
                        {
                            type: "min", name: '最小值'
                        }
                    ]
                },
                markLine:{  //  平均值
                    data:[
                        {
                            type: 'average', name: '平均值'
                        }
                    ]
                },
                label:{
                    show: true, //  是否可见
                    rotate: 60  //  是否旋转
                },
                barWidth: '30%' //  柱宽度
            }
        ]
    }
    <!-- 5. 将配置项设置给 echarts 实例对象 -->
    mCharts.setOption(option)
</script>
</body>
</html>
```
### 1.2 横向柱形图
```html
  xAxis: {
            type: 'value',
        },
        yAxis: {
            type: 'category',
            data: xDataArr
        },
```
将xAxis 和 yAxis 中的信息交换即可
![](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202303301912836.png)

### 1.3 通用配置
#### title【标题】
```html
        title: {
            text: "成绩", // 标题文字
            textStyle: {
                color: 'red' // 文字颜色
            },
            borderWidth: 5, // 标题边框
            borderColor: 'green', // 标题边框颜色
            borderRadius: 5, // 标题边框圆角
            left: 20, // 标题的位置
            top: 20 // 标题的位置
        }
```
#### tooltip【提示框 => 鼠标滑过或者点击】
tooltip 指的是当鼠标移入到图表或者点击图表时, 展示出的提示框
- 触发类型: trigger
```bash
可选值有item\axis
```
- 触发时机: triggerOn
```bash
可选值有 mousemove\click
```
- 格式化显示: formatter

模板：
```html
var option = {
  tooltip: {
  trigger: 'item',
  triggerOn: 'click',
  formatter: '{b}:{c}' <!-- 这个{b} 和 {c} 所代表的含义不需要去记, 在官方文档中有详细的描述 -->
  }
}
```
模板解释：
模板变量有：{a}、{b}、{c}、{d}、{e} 分别表示：系列名，数据名、数据值等
当 trigger 为 'axis' 时，会有多个系列的数据，此时可以通过 {a0}、{a1}、{a2} 这种后面加索引的方式表示系列的索引
不同图表类型下的 {a}、{b}、{c}、{d} 含义不一样，其中 {a}、{b}、{c}、{d} 在不同图表类型下代表数据含义为：
- 折线（区域）图，柱状（条形）图，K线图：{a}（系列名称）、{b}（类目值）、{c}（数值）、{d}（无）
- 散点图（气泡）图：{a}（系列名称）、{b}（数据名称）、{c}（数值数值）、{d}（无）
- 地图：{a}(系列名称)、{b}（区域名称）、{c}（合并数值）、{d}（无）
- 饼图、仪表盘、漏斗图：{a}（系列名称）、{b}（数据项名称）、{c}（数值）、{d}（百分比）

#### 回调函数
```js
        tooltip: {
            // trigger: 'item' ：鼠标移动到柱体
            trigger: 'axis', //  鼠标移到柱体内坐标轴
            triggerOn: 'click', //  默认是 mousemove
            // formatter: '{b} 的成绩是 {c}'
            formatter: function (arg) {    //  回调函数
                console.log(arg);
                return arg[0].name  + ' 的分数是 ' + arg[0].data;
            }
        }
```
![](https://img2023.cnblogs.com/blog/2171496/202303/2171496-20230319115023547-1704025256.png)
#### toolbox【工具框】
toolbox 是ECharts 提供的工具栏, 内置有 导出图片，数据视图, 重置, 数据区域缩放, 动态类型切换五个工具
工具栏的按钮是配置在feature 的节点之下
```js
        toolbox: {
            feature:{
                saveAsImage:{}, //  导出图片
                dataView: {},   //  数据视图
                restore: {}, // 重置
                dataZoom: {},    //  区域缩放
                magicType: {    //  动态图标类型切换
                    type:[  //  数组
                        'bar',
                        'line'
                    ]
                }
            }
        }
```
#### legend【筛选类别】
legend 是图例,用于筛选类别,需要和series 配合使用
- legend 中的data 是一个数组
- legend 中的 data 的值需要和series 数组中某组数据的name 值一致
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>柱形图入门</title>
    <!-- 1. 引入 echarts.js 文件-->
    <script src="lib/echarts.js"></script>

</head>
<body>
<!-- 2. 准备一个呈现图表的盒子 div -->
<div style="width: 600px;height: 400px"></div>

<script>
    // 3. 初始化 echarts 实例对象
    // 参数，dom，决定图表最终呈现的位置
    var mCharts = echarts.init(document.querySelector('div'));

    //  准备数据
    var xDataArr = ['张三', '李四', '王五', '闰土', '小明', '茅台', '二妞', '大强']
    var yDataArr1 = [88, 92, 63, 77, 94, 80, 72, 86]
    var yDataArr2 = [93, 60, 61, 62, 85, 79, 92, 30]


    <!-- 4. 准备配置项 -->
    var option = {
        title: {
            text: "成绩", // 标题文字
            textStyle: {
                color: 'red' // 文字颜色
            },
            borderWidth: 5, // 标题边框
            borderColor: 'blue', // 标题边框颜色
            borderRadius: 5, // 标题边框圆角
            left: 20, // 标题的位置
            top: 20 // 标题的位置
        },

        tooltip: {
            // trigger: 'item' ：鼠标移动到柱体
            trigger: 'axis', //  鼠标移到柱体内坐标轴
            triggerOn: 'click', //  默认是 mousemove
            // formatter: '{b} 的成绩是 {c}'
            formatter: function (arg) {    //  回调函数
                console.log(arg);
                return arg[0].name  + ' 的分数是 ' + arg[0].data;
            }
        },

        legend: {
            data: ['语文', '数学']  //  系列筛选 【和 series中的 name 保持一致】

        },

        xAxis: {
            type: 'category',
            data: xDataArr
        },
        yAxis: {
            type: 'value',

        },
        series: [
            {
                name: '语文',
                type: 'bar', // line：折线图
                data: yDataArr1,
                itemStyle:{
                    color: 'red'
                },
                markPoint:{ //  极值
                    data:[
                        {
                            type: 'max', name: '最大值'
                        },
                        {
                            type: "min", name: '最小值'
                        }
                    ]
                },
                markLine:{  //  平均值
                    data:[
                        {
                            type: 'average', name: '平均值'
                        }
                    ]
                },
                label:{
                    show: true, //  是否可见
                    rotate: 60  //  是否旋转
                },
                barWidth: '30%' //  柱宽度
            },

            {
                name: '数学',
                type: 'bar', // line：折线图
                data: yDataArr2,
                itemStyle:{
                    color: 'blue'
                },
                markPoint:{ //  极值
                    data:[
                        {
                            type: 'max', name: '最大值'
                        },
                        {
                            type: "min", name: '最小值'
                        }
                    ]
                },
                markLine:{  //  平均值
                    data:[
                        {
                            type: 'average', name: '平均值'
                        }
                    ]
                },
                label:{
                    show: true, //  是否可见
                    rotate: 60  //  是否旋转
                },
                barWidth: '30%' //  柱宽度
            }

        ]
    }
    <!-- 5. 将配置项设置给 echarts 实例对象 -->
    mCharts.setOption(option)
</script>
</body>
</html>
```
![](https://img2023.cnblogs.com/blog/2171496/202303/2171496-20230319123717017-225593412.png)

总结：
通用配置指的是：任何一种类型的图表都可以使用的配置

## 2. 折线图【line】
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>折线图入门</title>
    <script src="lib/echarts.js"></script>
</head>
<body>

<div style="width: 600px;height:400px"></div>

<script>
    var xDataArr = ['1月', '2月', '3月', '4月', '5月', '6月', '7月', '8月', '9月',
        '10月', '11月', '12月']
    var yDataArr = [3000, 2800, 900, 1000, 800, 700, 1400, 1300, 900, 1000, 800,
        600]
    var mCharts = echarts.init(document.querySelector("div"))
    var option = {
        xAxis: {
            type: 'category',
            data: xDataArr,
            boundaryGap: false  //  紧挨边缘
        },
        yAxis: {
            type: 'value',
            scale: true     //  摆脱 0 值比例
        },
        series: [
            {
                type: 'line',
                data: yDataArr,
                itemStyle:{
                    color: 'red'
                },
                smooth: true, //    线条平滑控制
                lineStyle: {    //  线条样式
                    color: 'green',
                    type: 'dashed' // 可选值还有 dotted solid
                },
                areaStyle: {    //  填充风格
                    color: 'green'
                },

                markPoint:{ //  极值
                    data:[
                        {
                            type: 'max', name: '最大值'
                        },
                        {
                            type: "min", name: '最小值'
                        }
                    ]
                },
                markLine:{  //  平均值
                    data:[
                        {
                            type: 'average', name: '平均值'
                        }
                    ]
                },
                markArea: { //  标注区间
                    data: [
                        [
                            {
                                xAxis: '1月'
                            }, {
                            xAxis: '2月'
                        }
                        ],
                        [
                            {
                                xAxis: '7月'
                            }, {
                            xAxis: '8月'
                        }
                        ]
                    ]
                }
            }
        ]
    }
    mCharts.setOption(option)
</script>
</body>
</html>
```
![](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202303301912711.png)
### 2.1 堆叠图【areaStyle: {}】
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>折线图入门</title>
    <script src="lib/echarts.js"></script>
</head>
<body>

<div style="width: 600px;height:400px"></div>

<script>
    var xDataArr = ['周一', '周二', '周三', '周四', '周五', '周六', '周日']
    var yDataArr1 = [120, 132, 101, 134, 90, 230, 210]
    var yDataArr2 = [20, 82, 191, 94, 290, 330, 310]

    var mCharts = echarts.init(document.querySelector("div"))
    var option = {
        xAxis: {
            type: 'category',
            data: xDataArr,
            boundaryGap: false
        },
        yAxis: {
            type: 'value',
            scale: true
        },
        series: [
            {
                type: 'line',
                data: yDataArr1,
                lineStyle: {    //  线条样式
                    color: 'red',
                    type: 'dashed' // 可选值还有 dotted solid
                },
                stack: 'all' // series中的每一个对象配置相同的stack值, 这个all可以任意写
            },
            {
                type: 'line',
                data: yDataArr2,
                lineStyle: {    //  线条样式
                    color: 'blue',
                    type: 'dashed' // 可选值还有 dotted solid
                },
                stack: 'all' // series中的每一个对象配置相同的stack值, 这个all可以任意写
            }
        ]
    }
    mCharts.setOption(option)
</script>
</body>
</html>
```
![](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202303301912820.png)
蓝色这条线的y轴起点, 不再是y轴, 而是红色这条线对应的点. 所以相当于蓝色是在红色这条线的基础之上进行绘制. 基于前一个图表进行堆叠
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>折线图入门</title>
    <script src="lib/echarts.js"></script>
</head>
<body>

<div style="width: 600px;height:400px"></div>

<script>
    var xDataArr = ['周一', '周二', '周三', '周四', '周五', '周六', '周日']
    var yDataArr1 = [120, 132, 101, 134, 90, 230, 210]
    var yDataArr2 = [20, 82, 191, 94, 290, 330, 310]

    var mCharts = echarts.init(document.querySelector("div"))
    var option = {
        xAxis: {
            type: 'category',
            data: xDataArr,
            boundaryGap: false
        },
        yAxis: {
            type: 'value',
            scale: true
        },
        series: [
            {
                type: 'line',
                data: yDataArr1,
                lineStyle: {    //  线条样式
                    color: 'red',
                    type: 'dashed' // 可选值还有 dotted solid
                },
                stack: 'all',// series中的每一个对象配置相同的stack值, 这个all可以任意写,
                areaStyle: {}
            },
            {
                type: 'line',
                data: yDataArr2,
                lineStyle: {    //  线条样式
                    color: 'blue',
                    type: 'dashed' // 可选值还有 dotted solid
                },
                stack: 'all', // series中的每一个对象配置相同的stack值, 这个all可以任意写
                areaStyle: {}
            }
        ]
    }
    mCharts.setOption(option)
</script>
</body>
</html>
```
![](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202303301912577.png)
### 2.2 特点
折线图更多的使用来呈现数据随时间的『变化趋势』
## 3. 散点图【scatter】
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>散点图</title>
    <script src="lib/echarts.js"></script>
</head>
<body>

<div style="width: 600px;height:400px"></div>

<script>
    // 1. x轴和y轴数据 二维数组 [ [身高,体重],...   ]
    // 2. 将 series 的 type 值设置为 scatter, x轴和y轴的type都是value
    var data = [{ "gender": "female", "height": 161.2, "weight": 51.6 }, { "gender": "female", "height": 167.5, "weight": 59 }, { "gender": "female", "height": 159.5, "weight": 49.2 }, { "gender": "female", "height": 157, "weight": 63 }, { "gender": "female", "height": 155.8, "weight": 53.6 }, { "gender": "female", "height": 170, "weight": 59 }, { "gender": "female", "height": 159.1, "weight": 47.6 }, { "gender": "female", "height": 166, "weight": 69.8 }, { "gender": "female", "height": 176.2, "weight": 66.8 }, { "gender": "female", "height": 160.2, "weight": 75.2 }, { "gender": "female", "height": 172.5, "weight": 55.2 }, { "gender": "female", "height": 170.9, "weight": 54.2 }, { "gender": "female", "height": 172.9, "weight": 62.5 }, { "gender": "female", "height": 153.4, "weight": 42 }, { "gender": "female", "height": 160, "weight": 50 }, { "gender": "female", "height": 147.2, "weight": 49.8 }, { "gender": "female", "height": 168.2, "weight": 49.2 }, { "gender": "female", "height": 175, "weight": 73.2 }, { "gender": "female", "height": 157, "weight": 47.8 }, { "gender": "female", "height": 167.6, "weight": 68.8 }, { "gender": "female", "height": 159.5, "weight": 50.6 }, { "gender": "female", "height": 175, "weight": 82.5 }, { "gender": "female", "height": 166.8, "weight": 57.2 }, { "gender": "female", "height": 176.5, "weight": 87.8 }, { "gender": "female", "height": 170.2, "weight": 72.8 }, { "gender": "female", "height": 174, "weight": 54.5 }, { "gender": "female", "height": 173, "weight": 59.8 }, { "gender": "female", "height": 179.9, "weight": 67.3 }, { "gender": "female", "height": 170.5, "weight": 67.8 }, { "gender": "female", "height": 160, "weight": 47 }, { "gender": "female", "height": 154.4, "weight": 46.2 }, { "gender": "female", "height": 162, "weight": 55 }, { "gender": "female", "height": 176.5, "weight": 83 }, { "gender": "female", "height": 160, "weight": 54.4 }, { "gender": "female", "height": 152, "weight": 45.8 }, { "gender": "female", "height": 162.1, "weight": 53.6 }, { "gender": "female", "height": 170, "weight": 73.2 }, { "gender": "female", "height": 160.2, "weight": 52.1 }, { "gender": "female", "height": 161.3, "weight": 67.9 }, { "gender": "female", "height": 166.4, "weight": 56.6 }, { "gender": "female", "height": 168.9, "weight": 62.3 }, { "gender": "female", "height": 163.8, "weight": 58.5 }, { "gender": "female", "height": 167.6, "weight": 54.5 }, { "gender": "female", "height": 160, "weight": 50.2 }, { "gender": "female", "height": 161.3, "weight": 60.3 }, { "gender": "female", "height": 167.6, "weight": 58.3 }, { "gender": "female", "height": 165.1, "weight": 56.2 }, { "gender": "female", "height": 160, "weight": 50.2 }, { "gender": "female", "height": 170, "weight": 72.9 }, { "gender": "female", "height": 157.5, "weight": 59.8 }, { "gender": "female", "height": 167.6, "weight": 61 }, { "gender": "female", "height": 160.7, "weight": 69.1 }, { "gender": "female", "height": 163.2, "weight": 55.9 }, { "gender": "female", "height": 152.4, "weight": 46.5 }, { "gender": "female", "height": 157.5, "weight": 54.3 }, { "gender": "female", "height": 168.3, "weight": 54.8 }, { "gender": "female", "height": 180.3, "weight": 60.7 }, { "gender": "female", "height": 165.5, "weight": 60 }, { "gender": "female", "height": 165, "weight": 62 }, { "gender": "female", "height": 164.5, "weight": 60.3 }, { "gender": "female", "height": 156, "weight": 52.7 }, { "gender": "female", "height": 160, "weight": 74.3 }, { "gender": "female", "height": 163, "weight": 62 }, { "gender": "female", "height": 165.7, "weight": 73.1 }, { "gender": "female", "height": 161, "weight": 80 }, { "gender": "female", "height": 162, "weight": 54.7 }, { "gender": "female", "height": 166, "weight": 53.2 }, { "gender": "female", "height": 174, "weight": 75.7 }, { "gender": "female", "height": 172.7, "weight": 61.1 }, { "gender": "female", "height": 167.6, "weight": 55.7 }, { "gender": "female", "height": 151.1, "weight": 48.7 }, { "gender": "female", "height": 164.5, "weight": 52.3 }, { "gender": "female", "height": 163.5, "weight": 50 }, { "gender": "female", "height": 152, "weight": 59.3 }, { "gender": "female", "height": 169, "weight": 62.5 }, { "gender": "female", "height": 164, "weight": 55.7 }, { "gender": "female", "height": 161.2, "weight": 54.8 }, { "gender": "female", "height": 155, "weight": 45.9 }, { "gender": "female", "height": 170, "weight": 70.6 }, { "gender": "female", "height": 176.2, "weight": 67.2 }, { "gender": "female", "height": 170, "weight": 69.4 }, { "gender": "female", "height": 162.5, "weight": 58.2 }, { "gender": "female", "height": 170.3, "weight": 64.8 }, { "gender": "female", "height": 164.1, "weight": 71.6 }, { "gender": "female", "height": 169.5, "weight": 52.8 }, { "gender": "female", "height": 163.2, "weight": 59.8 }, { "gender": "female", "height": 154.5, "weight": 49 }, { "gender": "female", "height": 159.8, "weight": 50 }, { "gender": "female", "height": 173.2, "weight": 69.2 }, { "gender": "female", "height": 170, "weight": 55.9 }, { "gender": "female", "height": 161.4, "weight": 63.4 }, { "gender": "female", "height": 169, "weight": 58.2 }, { "gender": "female", "height": 166.2, "weight": 58.6 }, { "gender": "female", "height": 159.4, "weight": 45.7 }, { "gender": "female", "height": 162.5, "weight": 52.2 }, { "gender": "female", "height": 159, "weight": 48.6 }, { "gender": "female", "height": 162.8, "weight": 57.8 }, { "gender": "female", "height": 159, "weight": 55.6 }, { "gender": "female", "height": 179.8, "weight": 66.8 }, { "gender": "female", "height": 162.9, "weight": 59.4 }, { "gender": "female", "height": 161, "weight": 53.6 }, { "gender": "female", "height": 151.1, "weight": 73.2 }, { "gender": "female", "height": 168.2, "weight": 53.4 }, { "gender": "female", "height": 168.9, "weight": 69 }, { "gender": "female", "height": 173.2, "weight": 58.4 }, { "gender": "female", "height": 171.8, "weight": 56.2 }, { "gender": "female", "height": 178, "weight": 70.6 }, { "gender": "female", "height": 164.3, "weight": 59.8 }, { "gender": "female", "height": 163, "weight": 72 }, { "gender": "female", "height": 168.5, "weight": 65.2 }, { "gender": "female", "height": 166.8, "weight": 56.6 }, { "gender": "female", "height": 172.7, "weight": 105.2 }, { "gender": "female", "height": 163.5, "weight": 51.8 }, { "gender": "female", "height": 169.4, "weight": 63.4 }, { "gender": "female", "height": 167.8, "weight": 59 }, { "gender": "female", "height": 159.5, "weight": 47.6 }, { "gender": "female", "height": 167.6, "weight": 63 }, { "gender": "female", "height": 161.2, "weight": 55.2 }, { "gender": "female", "height": 160, "weight": 45 }, { "gender": "female", "height": 163.2, "weight": 54 }, { "gender": "female", "height": 162.2, "weight": 50.2 }, { "gender": "female", "height": 161.3, "weight": 60.2 }, { "gender": "female", "height": 149.5, "weight": 44.8 }, { "gender": "female", "height": 157.5, "weight": 58.8 }, { "gender": "female", "height": 163.2, "weight": 56.4 }, { "gender": "female", "height": 172.7, "weight": 62 }, { "gender": "female", "height": 155, "weight": 49.2 }, { "gender": "female", "height": 156.5, "weight": 67.2 }, { "gender": "female", "height": 164, "weight": 53.8 }, { "gender": "female", "height": 160.9, "weight": 54.4 }, { "gender": "female", "height": 162.8, "weight": 58 }, { "gender": "female", "height": 167, "weight": 59.8 }, { "gender": "female", "height": 160, "weight": 54.8 }, { "gender": "female", "height": 160, "weight": 43.2 }, { "gender": "female", "height": 168.9, "weight": 60.5 }, { "gender": "female", "height": 158.2, "weight": 46.4 }, { "gender": "female", "height": 156, "weight": 64.4 }, { "gender": "female", "height": 160, "weight": 48.8 }, { "gender": "female", "height": 167.1, "weight": 62.2 }, { "gender": "female", "height": 158, "weight": 55.5 }, { "gender": "female", "height": 167.6, "weight": 57.8 }, { "gender": "female", "height": 156, "weight": 54.6 }, { "gender": "female", "height": 162.1, "weight": 59.2 }, { "gender": "female", "height": 173.4, "weight": 52.7 }, { "gender": "female", "height": 159.8, "weight": 53.2 }, { "gender": "female", "height": 170.5, "weight": 64.5 }, { "gender": "female", "height": 159.2, "weight": 51.8 }, { "gender": "female", "height": 157.5, "weight": 56 }, { "gender": "female", "height": 161.3, "weight": 63.6 }, { "gender": "female", "height": 162.6, "weight": 63.2 }, { "gender": "female", "height": 160, "weight": 59.5 }, { "gender": "female", "height": 168.9, "weight": 56.8 }, { "gender": "female", "height": 165.1, "weight": 64.1 }, { "gender": "female", "height": 162.6, "weight": 50 }, { "gender": "female", "height": 165.1, "weight": 72.3 }, { "gender": "female", "height": 166.4, "weight": 55 }, { "gender": "female", "height": 160, "weight": 55.9 }, { "gender": "female", "height": 152.4, "weight": 60.4 }, { "gender": "female", "height": 170.2, "weight": 69.1 }, { "gender": "female", "height": 162.6, "weight": 84.5 }, { "gender": "female", "height": 170.2, "weight": 55.9 }, { "gender": "female", "height": 158.8, "weight": 55.5 }, { "gender": "female", "height": 172.7, "weight": 69.5 }, { "gender": "female", "height": 167.6, "weight": 76.4 }, { "gender": "female", "height": 162.6, "weight": 61.4 }, { "gender": "female", "height": 167.6, "weight": 65.9 }, { "gender": "female", "height": 156.2, "weight": 58.6 }, { "gender": "female", "height": 175.2, "weight": 66.8 }, { "gender": "female", "height": 172.1, "weight": 56.6 }, { "gender": "female", "height": 162.6, "weight": 58.6 }, { "gender": "female", "height": 160, "weight": 55.9 }, { "gender": "female", "height": 165.1, "weight": 59.1 }, { "gender": "female", "height": 182.9, "weight": 81.8 }, { "gender": "female", "height": 166.4, "weight": 70.7 }, { "gender": "female", "height": 165.1, "weight": 56.8 }, { "gender": "female", "height": 177.8, "weight": 60 }, { "gender": "female", "height": 165.1, "weight": 58.2 }, { "gender": "female", "height": 175.3, "weight": 72.7 }, { "gender": "female", "height": 154.9, "weight": 54.1 }, { "gender": "female", "height": 158.8, "weight": 49.1 }, { "gender": "female", "height": 172.7, "weight": 75.9 }, { "gender": "female", "height": 168.9, "weight": 55 }, { "gender": "female", "height": 161.3, "weight": 57.3 }, { "gender": "female", "height": 167.6, "weight": 55 }, { "gender": "female", "height": 165.1, "weight": 65.5 }, { "gender": "female", "height": 175.3, "weight": 65.5 }, { "gender": "female", "height": 157.5, "weight": 48.6 }, { "gender": "female", "height": 163.8, "weight": 58.6 }, { "gender": "female", "height": 167.6, "weight": 63.6 }, { "gender": "female", "height": 165.1, "weight": 55.2 }, { "gender": "female", "height": 165.1, "weight": 62.7 }, { "gender": "female", "height": 168.9, "weight": 56.6 }, { "gender": "female", "height": 162.6, "weight": 53.9 }, { "gender": "female", "height": 164.5, "weight": 63.2 }, { "gender": "female", "height": 176.5, "weight": 73.6 }, { "gender": "female", "height": 168.9, "weight": 62 }, { "gender": "female", "height": 175.3, "weight": 63.6 }, { "gender": "female", "height": 159.4, "weight": 53.2 }, { "gender": "female", "height": 160, "weight": 53.4 }, { "gender": "female", "height": 170.2, "weight": 55 }, { "gender": "female", "height": 162.6, "weight": 70.5 }, { "gender": "female", "height": 167.6, "weight": 54.5 }, { "gender": "female", "height": 162.6, "weight": 54.5 }, { "gender": "female", "height": 160.7, "weight": 55.9 }, { "gender": "female", "height": 160, "weight": 59 }, { "gender": "female", "height": 157.5, "weight": 63.6 }, { "gender": "female", "height": 162.6, "weight": 54.5 }, { "gender": "female", "height": 152.4, "weight": 47.3 }, { "gender": "female", "height": 170.2, "weight": 67.7 }, { "gender": "female", "height": 165.1, "weight": 80.9 }, { "gender": "female", "height": 172.7, "weight": 70.5 }, { "gender": "female", "height": 165.1, "weight": 60.9 }, { "gender": "female", "height": 170.2, "weight": 63.6 }, { "gender": "female", "height": 170.2, "weight": 54.5 }, { "gender": "female", "height": 170.2, "weight": 59.1 }, { "gender": "female", "height": 161.3, "weight": 70.5 }, { "gender": "female", "height": 167.6, "weight": 52.7 }, { "gender": "female", "height": 167.6, "weight": 62.7 }, { "gender": "female", "height": 165.1, "weight": 86.3 }, { "gender": "female", "height": 162.6, "weight": 66.4 }, { "gender": "female", "height": 152.4, "weight": 67.3 }, { "gender": "female", "height": 168.9, "weight": 63 }, { "gender": "female", "height": 170.2, "weight": 73.6 }, { "gender": "female", "height": 175.2, "weight": 62.3 }, { "gender": "female", "height": 175.2, "weight": 57.7 }, { "gender": "female", "height": 160, "weight": 55.4 }, { "gender": "female", "height": 165.1, "weight": 104.1 }, { "gender": "female", "height": 174, "weight": 55.5 }, { "gender": "female", "height": 170.2, "weight": 77.3 }, { "gender": "female", "height": 160, "weight": 80.5 }, { "gender": "female", "height": 167.6, "weight": 64.5 }, { "gender": "female", "height": 167.6, "weight": 72.3 }, { "gender": "female", "height": 167.6, "weight": 61.4 }, { "gender": "female", "height": 154.9, "weight": 58.2 }, { "gender": "female", "height": 162.6, "weight": 81.8 }, { "gender": "female", "height": 175.3, "weight": 63.6 }, { "gender": "female", "height": 171.4, "weight": 53.4 }, { "gender": "female", "height": 157.5, "weight": 54.5 }, { "gender": "female", "height": 165.1, "weight": 53.6 }, { "gender": "female", "height": 160, "weight": 60 }, { "gender": "female", "height": 174, "weight": 73.6 }, { "gender": "female", "height": 162.6, "weight": 61.4 }, { "gender": "female", "height": 174, "weight": 55.5 }, { "gender": "female", "height": 162.6, "weight": 63.6 }, { "gender": "female", "height": 161.3, "weight": 60.9 }, { "gender": "female", "height": 156.2, "weight": 60 }, { "gender": "female", "height": 149.9, "weight": 46.8 }, { "gender": "female", "height": 169.5, "weight": 57.3 }, { "gender": "female", "height": 160, "weight": 64.1 }, { "gender": "female", "height": 175.3, "weight": 63.6 }, { "gender": "female", "height": 169.5, "weight": 67.3 }, { "gender": "female", "height": 160, "weight": 75.5 }, { "gender": "female", "height": 172.7, "weight": 68.2 }, { "gender": "female", "height": 162.6, "weight": 61.4 }, { "gender": "female", "height": 157.5, "weight": 76.8 }, { "gender": "female", "height": 176.5, "weight": 71.8 }, { "gender": "female", "height": 164.4, "weight": 55.5 }, { "gender": "female", "height": 160.7, "weight": 48.6 }, { "gender": "female", "height": 174, "weight": 66.4 }, { "gender": "female", "height": 163.8, "weight": 67.3 }, { "gender": "male", "height": 174, "weight": 65.6 }, { "gender": "male", "height": 175.3, "weight": 71.8 }, { "gender": "male", "height": 193.5, "weight": 80.7 }, { "gender": "male", "height": 186.5, "weight": 72.6 }, { "gender": "male", "height": 187.2, "weight": 78.8 }, { "gender": "male", "height": 181.5, "weight": 74.8 }, { "gender": "male", "height": 184, "weight": 86.4 }, { "gender": "male", "height": 184.5, "weight": 78.4 }, { "gender": "male", "height": 175, "weight": 62 }, { "gender": "male", "height": 184, "weight": 81.6 }, { "gender": "male", "height": 180, "weight": 76.6 }, { "gender": "male", "height": 177.8, "weight": 83.6 }, { "gender": "male", "height": 192, "weight": 90 }, { "gender": "male", "height": 176, "weight": 74.6 }, { "gender": "male", "height": 174, "weight": 71 }, { "gender": "male", "height": 184, "weight": 79.6 }, { "gender": "male", "height": 192.7, "weight": 93.8 }, { "gender": "male", "height": 171.5, "weight": 70 }, { "gender": "male", "height": 173, "weight": 72.4 }, { "gender": "male", "height": 176, "weight": 85.9 }, { "gender": "male", "height": 176, "weight": 78.8 }, { "gender": "male", "height": 180.5, "weight": 77.8 }, { "gender": "male", "height": 172.7, "weight": 66.2 }, { "gender": "male", "height": 176, "weight": 86.4 }, { "gender": "male", "height": 173.5, "weight": 81.8 }, { "gender": "male", "height": 178, "weight": 89.6 }, { "gender": "male", "height": 180.3, "weight": 82.8 }, { "gender": "male", "height": 180.3, "weight": 76.4 }, { "gender": "male", "height": 164.5, "weight": 63.2 }, { "gender": "male", "height": 173, "weight": 60.9 }, { "gender": "male", "height": 183.5, "weight": 74.8 }, { "gender": "male", "height": 175.5, "weight": 70 }, { "gender": "male", "height": 188, "weight": 72.4 }, { "gender": "male", "height": 189.2, "weight": 84.1 }, { "gender": "male", "height": 172.8, "weight": 69.1 }, { "gender": "male", "height": 170, "weight": 59.5 }, { "gender": "male", "height": 182, "weight": 67.2 }, { "gender": "male", "height": 170, "weight": 61.3 }, { "gender": "male", "height": 177.8, "weight": 68.6 }, { "gender": "male", "height": 184.2, "weight": 80.1 }, { "gender": "male", "height": 186.7, "weight": 87.8 }, { "gender": "male", "height": 171.4, "weight": 84.7 }, { "gender": "male", "height": 172.7, "weight": 73.4 }, { "gender": "male", "height": 175.3, "weight": 72.1 }, { "gender": "male", "height": 180.3, "weight": 82.6 }, { "gender": "male", "height": 182.9, "weight": 88.7 }, { "gender": "male", "height": 188, "weight": 84.1 }, { "gender": "male", "height": 177.2, "weight": 94.1 }, { "gender": "male", "height": 172.1, "weight": 74.9 }, { "gender": "male", "height": 167, "weight": 59.1 }, { "gender": "male", "height": 169.5, "weight": 75.6 }, { "gender": "male", "height": 174, "weight": 86.2 }, { "gender": "male", "height": 172.7, "weight": 75.3 }, { "gender": "male", "height": 182.2, "weight": 87.1 }, { "gender": "male", "height": 164.1, "weight": 55.2 }, { "gender": "male", "height": 163, "weight": 57 }, { "gender": "male", "height": 171.5, "weight": 61.4 }, { "gender": "male", "height": 184.2, "weight": 76.8 }, { "gender": "male", "height": 174, "weight": 86.8 }, { "gender": "male", "height": 174, "weight": 72.2 }, { "gender": "male", "height": 177, "weight": 71.6 }, { "gender": "male", "height": 186, "weight": 84.8 }, { "gender": "male", "height": 167, "weight": 68.2 }, { "gender": "male", "height": 171.8, "weight": 66.1 }, { "gender": "male", "height": 182, "weight": 72 }, { "gender": "male", "height": 167, "weight": 64.6 }, { "gender": "male", "height": 177.8, "weight": 74.8 }, { "gender": "male", "height": 164.5, "weight": 70 }, { "gender": "male", "height": 192, "weight": 101.6 }, { "gender": "male", "height": 175.5, "weight": 63.2 }, { "gender": "male", "height": 171.2, "weight": 79.1 }, { "gender": "male", "height": 181.6, "weight": 78.9 }, { "gender": "male", "height": 167.4, "weight": 67.7 }, { "gender": "male", "height": 181.1, "weight": 66 }, { "gender": "male", "height": 177, "weight": 68.2 }, { "gender": "male", "height": 174.5, "weight": 63.9 }, { "gender": "male", "height": 177.5, "weight": 72 }, { "gender": "male", "height": 170.5, "weight": 56.8 }, { "gender": "male", "height": 182.4, "weight": 74.5 }, { "gender": "male", "height": 197.1, "weight": 90.9 }, { "gender": "male", "height": 180.1, "weight": 93 }, { "gender": "male", "height": 175.5, "weight": 80.9 }, { "gender": "male", "height": 180.6, "weight": 72.7 }, { "gender": "male", "height": 184.4, "weight": 68 }, { "gender": "male", "height": 175.5, "weight": 70.9 }, { "gender": "male", "height": 180.6, "weight": 72.5 }, { "gender": "male", "height": 177, "weight": 72.5 }, { "gender": "male", "height": 177.1, "weight": 83.4 }, { "gender": "male", "height": 181.6, "weight": 75.5 }, { "gender": "male", "height": 176.5, "weight": 73 }, { "gender": "male", "height": 175, "weight": 70.2 }, { "gender": "male", "height": 174, "weight": 73.4 }, { "gender": "male", "height": 165.1, "weight": 70.5 }, { "gender": "male", "height": 177, "weight": 68.9 }, { "gender": "male", "height": 192, "weight": 102.3 }, { "gender": "male", "height": 176.5, "weight": 68.4 }, { "gender": "male", "height": 169.4, "weight": 65.9 }, { "gender": "male", "height": 182.1, "weight": 75.7 }, { "gender": "male", "height": 179.8, "weight": 84.5 }, { "gender": "male", "height": 175.3, "weight": 87.7 }, { "gender": "male", "height": 184.9, "weight": 86.4 }, { "gender": "male", "height": 177.3, "weight": 73.2 }, { "gender": "male", "height": 167.4, "weight": 53.9 }, { "gender": "male", "height": 178.1, "weight": 72 }, { "gender": "male", "height": 168.9, "weight": 55.5 }, { "gender": "male", "height": 157.2, "weight": 58.4 }, { "gender": "male", "height": 180.3, "weight": 83.2 }, { "gender": "male", "height": 170.2, "weight": 72.7 }, { "gender": "male", "height": 177.8, "weight": 64.1 }, { "gender": "male", "height": 172.7, "weight": 72.3 }, { "gender": "male", "height": 165.1, "weight": 65 }, { "gender": "male", "height": 186.7, "weight": 86.4 }, { "gender": "male", "height": 165.1, "weight": 65 }, { "gender": "male", "height": 174, "weight": 88.6 }, { "gender": "male", "height": 175.3, "weight": 84.1 }, { "gender": "male", "height": 185.4, "weight": 66.8 }, { "gender": "male", "height": 177.8, "weight": 75.5 }, { "gender": "male", "height": 180.3, "weight": 93.2 }, { "gender": "male", "height": 180.3, "weight": 82.7 }, { "gender": "male", "height": 177.8, "weight": 58 }, { "gender": "male", "height": 177.8, "weight": 79.5 }, { "gender": "male", "height": 177.8, "weight": 78.6 }, { "gender": "male", "height": 177.8, "weight": 71.8 }, { "gender": "male", "height": 177.8, "weight": 116.4 }, { "gender": "male", "height": 163.8, "weight": 72.2 }, { "gender": "male", "height": 188, "weight": 83.6 }, { "gender": "male", "height": 198.1, "weight": 85.5 }, { "gender": "male", "height": 175.3, "weight": 90.9 }, { "gender": "male", "height": 166.4, "weight": 85.9 }, { "gender": "male", "height": 190.5, "weight": 89.1 }, { "gender": "male", "height": 166.4, "weight": 75 }, { "gender": "male", "height": 177.8, "weight": 77.7 }, { "gender": "male", "height": 179.7, "weight": 86.4 }, { "gender": "male", "height": 172.7, "weight": 90.9 }, { "gender": "male", "height": 190.5, "weight": 73.6 }, { "gender": "male", "height": 185.4, "weight": 76.4 }, { "gender": "male", "height": 168.9, "weight": 69.1 }, { "gender": "male", "height": 167.6, "weight": 84.5 }, { "gender": "male", "height": 175.3, "weight": 64.5 }, { "gender": "male", "height": 170.2, "weight": 69.1 }, { "gender": "male", "height": 190.5, "weight": 108.6 }, { "gender": "male", "height": 177.8, "weight": 86.4 }, { "gender": "male", "height": 190.5, "weight": 80.9 }, { "gender": "male", "height": 177.8, "weight": 87.7 }, { "gender": "male", "height": 184.2, "weight": 94.5 }, { "gender": "male", "height": 176.5, "weight": 80.2 }, { "gender": "male", "height": 177.8, "weight": 72 }, { "gender": "male", "height": 180.3, "weight": 71.4 }, { "gender": "male", "height": 171.4, "weight": 72.7 }, { "gender": "male", "height": 172.7, "weight": 84.1 }, { "gender": "male", "height": 172.7, "weight": 76.8 }, { "gender": "male", "height": 177.8, "weight": 63.6 }, { "gender": "male", "height": 177.8, "weight": 80.9 }, { "gender": "male", "height": 182.9, "weight": 80.9 }, { "gender": "male", "height": 170.2, "weight": 85.5 }, { "gender": "male", "height": 167.6, "weight": 68.6 }, { "gender": "male", "height": 175.3, "weight": 67.7 }, { "gender": "male", "height": 165.1, "weight": 66.4 }, { "gender": "male", "height": 185.4, "weight": 102.3 }, { "gender": "male", "height": 181.6, "weight": 70.5 }, { "gender": "male", "height": 172.7, "weight": 95.9 }, { "gender": "male", "height": 190.5, "weight": 84.1 }, { "gender": "male", "height": 179.1, "weight": 87.3 }, { "gender": "male", "height": 175.3, "weight": 71.8 }, { "gender": "male", "height": 170.2, "weight": 65.9 }, { "gender": "male", "height": 193, "weight": 95.9 }, { "gender": "male", "height": 171.4, "weight": 91.4 }, { "gender": "male", "height": 177.8, "weight": 81.8 }, { "gender": "male", "height": 177.8, "weight": 96.8 }, { "gender": "male", "height": 167.6, "weight": 69.1 }, { "gender": "male", "height": 167.6, "weight": 82.7 }, { "gender": "male", "height": 180.3, "weight": 75.5 }, { "gender": "male", "height": 182.9, "weight": 79.5 }, { "gender": "male", "height": 176.5, "weight": 73.6 }, { "gender": "male", "height": 186.7, "weight": 91.8 }, { "gender": "male", "height": 188, "weight": 84.1 }, { "gender": "male", "height": 188, "weight": 85.9 }, { "gender": "male", "height": 177.8, "weight": 81.8 }, { "gender": "male", "height": 174, "weight": 82.5 }, { "gender": "male", "height": 177.8, "weight": 80.5 }, { "gender": "male", "height": 171.4, "weight": 70 }, { "gender": "male", "height": 185.4, "weight": 81.8 }, { "gender": "male", "height": 185.4, "weight": 84.1 }, { "gender": "male", "height": 188, "weight": 90.5 }, { "gender": "male", "height": 188, "weight": 91.4 }, { "gender": "male", "height": 182.9, "weight": 89.1 }, { "gender": "male", "height": 176.5, "weight": 85 }, { "gender": "male", "height": 175.3, "weight": 69.1 }, { "gender": "male", "height": 175.3, "weight": 73.6 }, { "gender": "male", "height": 188, "weight": 80.5 }, { "gender": "male", "height": 188, "weight": 82.7 }, { "gender": "male", "height": 175.3, "weight": 86.4 }, { "gender": "male", "height": 170.5, "weight": 67.7 }, { "gender": "male", "height": 179.1, "weight": 92.7 }, { "gender": "male", "height": 177.8, "weight": 93.6 }, { "gender": "male", "height": 175.3, "weight": 70.9 }, { "gender": "male", "height": 182.9, "weight": 75 }, { "gender": "male", "height": 170.8, "weight": 93.2 }, { "gender": "male", "height": 188, "weight": 93.2 }, { "gender": "male", "height": 180.3, "weight": 77.7 }, { "gender": "male", "height": 177.8, "weight": 61.4 }, { "gender": "male", "height": 185.4, "weight": 94.1 }, { "gender": "male", "height": 168.9, "weight": 75 }, { "gender": "male", "height": 185.4, "weight": 83.6 }, { "gender": "male", "height": 180.3, "weight": 85.5 }, { "gender": "male", "height": 174, "weight": 73.9 }, { "gender": "male", "height": 167.6, "weight": 66.8 }, { "gender": "male", "height": 182.9, "weight": 87.3 }, { "gender": "male", "height": 160, "weight": 72.3 }, { "gender": "male", "height": 180.3, "weight": 88.6 }, { "gender": "male", "height": 167.6, "weight": 75.5 }, { "gender": "male", "height": 186.7, "weight": 101.4 }, { "gender": "male", "height": 175.3, "weight": 91.1 }, { "gender": "male", "height": 175.3, "weight": 67.3 }, { "gender": "male", "height": 175.9, "weight": 77.7 }, { "gender": "male", "height": 175.3, "weight": 81.8 }, { "gender": "male", "height": 179.1, "weight": 75.5 }, { "gender": "male", "height": 181.6, "weight": 84.5 }, { "gender": "male", "height": 177.8, "weight": 76.6 }, { "gender": "male", "height": 182.9, "weight": 85 }, { "gender": "male", "height": 177.8, "weight": 102.5 }, { "gender": "male", "height": 184.2, "weight": 77.3 }, { "gender": "male", "height": 179.1, "weight": 71.8 }, { "gender": "male", "height": 176.5, "weight": 87.9 }, { "gender": "male", "height": 188, "weight": 94.3 }, { "gender": "male", "height": 174, "weight": 70.9 }, { "gender": "male", "height": 167.6, "weight": 64.5 }, { "gender": "male", "height": 170.2, "weight": 77.3 }, { "gender": "male", "height": 167.6, "weight": 72.3 }, { "gender": "male", "height": 188, "weight": 87.3 }, { "gender": "male", "height": 174, "weight": 80 }, { "gender": "male", "height": 176.5, "weight": 82.3 }, { "gender": "male", "height": 180.3, "weight": 73.6 }, { "gender": "male", "height": 167.6, "weight": 74.1 }, { "gender": "male", "height": 188, "weight": 85.9 }, { "gender": "male", "height": 180.3, "weight": 73.2 }, { "gender": "male", "height": 167.6, "weight": 76.3 }, { "gender": "male", "height": 183, "weight": 65.9 }, { "gender": "male", "height": 183, "weight": 90.9 }, { "gender": "male", "height": 179.1, "weight": 89.1 }, { "gender": "male", "height": 170.2, "weight": 62.3 }, { "gender": "male", "height": 177.8, "weight": 82.7 }, { "gender": "male", "height": 179.1, "weight": 79.1 }, { "gender": "male", "height": 190.5, "weight": 98.2 }, { "gender": "male", "height": 177.8, "weight": 84.1 }, { "gender": "male", "height": 180.3, "weight": 83.2 }, { "gender": "male", "height": 180.3, "weight": 83.2 }]
    var axisData = []

    for( var i=0;i<data.length;i++) {
        var height = data[i].height
        var weight = data[i].weight
        var newArr = [height, weight]
        axisData.push(newArr)
    }

    var mCharts = echarts.init(document.querySelector("div"))
    var option = {
        xAxis: {
            type: 'value',
            scale: true //  脱离 0 值比例
        },
        yAxis: {
            type: 'value',
            scale: true
        },
        series: [
            {   type: 'scatter', // 指明图表的类型为散点图
                data: axisData
            }
        ]
    }
    mCharts.setOption(option)
</script>
</body>
</html>
```
![](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202303301913001.png)
### 3.1 大小、颜色
```html
        series: [
            {   type: 'scatter', // 指明图表的类型为散点图
                data: axisData,
                symbolSize: 10,
                itemStyle: {
                    color: 'green',
                }
            }
        ]

```
### 3.2 回调函数
```html
        series: [
            {
                type: 'scatter',
                data: axisData,
                symbolSize: function (arg) {
                    var weight = arg[1]
                    var height = arg[0] / 100
                // BMI > 28 则代表肥胖, 肥胖的人用大的散点标识, 正常的人用小散点标识
                // BMI: 体重/ 身高*身高 kg m
                    var bmi = weight / (height * height)
                    if (bmi > 28) {
                        return 20
                    }
                    return 5
                },
                itemStyle: {
                    color: function (arg) {
                        var weight = arg.data[1]
                        var height = arg.data[0] / 100
                        var bmi = weight / (height * height)
                        if (bmi > 28) {
                            return 'red'
                        }
                        return 'green'
                    }
                }
            }
        ]

```
![](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202303301913494.png)
### 3.3 涟漪动画效果
```js
  type: 'effectScatter',
  rippleEffect:{  配置涟漪动画的大小
    scale:3
   },
  showEffectOn: 'emphasis',  //  可以控制涟漪动画在什么时候产生, 它的可选值有两个: render【界面渲染完成就开始涟漪动画】和 emphasis【鼠标移过某个散点的时候, 该散点开始涟漪动画】
```
![](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202303301913467.png)
散点图也经常结合地图来进行地图区域的标注, 这个效果将在讲解地图时实现
### 3.4 直角坐标系的常见配置
直角坐标系的图表指的是带有x轴和y轴的图表, 常见的直角坐标系的图表有: 柱状图 折线图 散点图
针对于直角坐标系的图表, 有一些通用的配置
1. 网格 grid
- 显示grid
show: true
- grid 的边框
borderWidth : 10
- grid 的位置和大小
left top right bottom
width height
```js
var option = {
    grid: {
        show: true, // 显示grid
        borderWidth: 10, // grid的边框宽度
        borderColor: 'red', // grid的边框颜色
        left: 100, // grid的位置
        top: 100,
        width: 300, // grid的大小
        height: 150
    }
}
```
2. 坐标轴 axis
坐标轴分为x轴和y轴, 一个grid 中最多有两种位置的x 轴和y 轴
- 坐标轴类型 type
value : 数值轴, 自动会从目标数据中读取数据
category : 类目轴, 该类型必须通过data 设置类目数据
- 坐标轴位置
xAxis : 可取值为 top 或者bottom
yAxis : 可取值为left 或者right
```js
var option = {
    xAxis: {
        type: 'category',
        data: xDataArr,
        position: 'top'
    },
    yAxis: {
        type: 'value',
        position: 'right'
    }
}
```
3. 区域缩放 dataZoom
dataZoom 用于区域缩放, 对数据范围过滤, x轴和y轴都可以拥有, dataZoom 是一个数组, 意味着可以配置多个区域缩放器
- 区域缩放类型 type
slider : 滑块
inside : 内置, 依靠鼠标滚轮或者双指缩放
- 产生作用的轴
xAxisIndex :设置缩放组件控制的是哪个x 轴, 一般写0即可
yAxisIndex :设置缩放组件控制的是哪个y 轴, 一般写0即可
- 指明初始状态的缩放情况
start : 数据窗口范围的起始百分比
end : 数据窗口范围的结束百分比
```js
var option = {
    xAxis: {
        type: 'category',
        data: xDataArr
    },
    yAxis: {
        type: 'value'
    },
    dataZoom: [
        {
            type: 'slider',
            xAxisIndex: 0
        },
        {
            type: 'slider',
            yAxisIndex: 0,
            start: 0,
            end: 80
        }
    ]
}
```