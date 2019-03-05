# 组件列表
高优先级
```
文字，查询，
线图，堆积线图，面积图，堆积面积图
柱图 ，堆积柱图，条形图，堆积条形图,簇状柱状图，对比柱状图，百分比堆积柱状图/条形图，双y轴
饼图
交叉表
气泡地图 色块地图
指标看板
漏斗图
雷达图
```
     
    
低优先级
```
tab
内嵌页面
查询控件新增需求待讨论
图片
icon库
目标漏斗图
商家后台雷达图
分区柱状图/折线图
对比柱状图/条形图
```
+ ## 视图类控件
``` 
包括 ：用echarts渲染的canvas控件
       表格，指标看板等纯数据展示控件
       这些控件都有 样式设置，高级（多表联动），数据列设置等选项
``` 
#### 视图类控件内部实现  
    ##### created()
    ```
获取series中的data数据部分(这里永远都只会是模版中的data数据)，缓存到组件内部的变量中
    ```
    ##### mounted()
    ```
添加组件时： mounted生命周期生成的是控件,同时监听点击事件，为图表联动做准备
    ```
    ##### createChart(type) type:init,search,self 
```     
 每个控件都有一个 createChart方法（type） init,search,self     
 这个方法的调用时机有：
            更新报表时： F5刷新页面时会调用 type：init 条件 跟查询控件关联的查询条件 以及自身的默认值字段
            查询时   ： 调用 type:search 条件，与查询控件关联的图表字段的条件
            图表联动时： 当点击图表时触发，type:search 查找与此图表关联的字段，
                       把点击图表获得的值赋给关联的图表，被关联的图表把这个值作为条件，调用 createChart放法               
            重置数据时： 相当于当前图表断开与其他图表的关联联系，type:self
                       只拿自己本身的指标/维度  ，过滤，排序作为条件查询
            更新数据时： 数据区点击更新按钮时，调用 type:self 条件同重置数据
```
##### 导出数据
```
导出数据这里使用到的是，当前图表拥有的 维度/指标/过滤/排序/字段
```        
##### watch的处理
```
 watch: {
    'info.options': {
      deep: true,
      handler(val) {
        if (val) {
          this.changeStyle();
        }
      },
    },
  methods:{
    changheStyle(){
        const options=cloneDeep(info.options)
        this.myChart.setOption(options, true);

    }
  }  
  changeStyle中的options必须深拷贝,否则会死循环
```

     
 
+ ## 特殊控件
        包括 ： 查询控件（此控件无样式设置，无高级部分）
               纯文本控件(此控件无样式设置，无高级部分,无数据设置部分，纯粹输入文字，所有的样式使用 
               tinymce富文本插件指定)
+ ## 查询控件
```
    查询控件有个contanerChart的概念，它是一个虚拟的控件，不会在页面中渲染，它只是用来描述 查询控件中 
查询组件与查询控件的关系,(查询组件即containerChart，它的存在使的一个查询控件中可以有多个数据集同时
存在)
    这样设计的原因是，像 quickbi 中的查询控件，它只能使用一个数据集，没有多个数据集的概念，
但是这里通过 containerChart，使得查询控件相对灵活
{
	"repChartModel": {
		"chartId": 15517814732, //当前containerChart的id
		"dataSetId": 230, //当前选中的 数据集id
		"repId": 0, 
		"beDel": false,
		"subType": "container",
		"title": "容器控件",
		"type": "container"
	},
	"fieldModels": [{
		"id": 0,
		"dataSetId": 230,
		"field": "区域",
		"alias": "区域",
		"type": 5,
		"fieldType": "String",
		"fieldId": 441,
		"beDel": false,
		"sortType": "",
		"isEditAlias": false,
		"opt": "NULL_OPT",
		"config": {
			"filterType": "enumeration",
			"fieldType": "String",
			"filterModels": [{
				"value": "华北",
				"filterCondition": "EXACT_MATCH",
				"combineType": "OR"
			}]
		}
	}],
	"relationModels": [{
		"id": 0,
		"chartId": 15517814732, // 关联的查询控价id
		"relateChartId": "15517814063", // 与查询控件关联的图表控件id
		"relationTypeId": 0,
		"relations": {
			"441": [359]关联关系
		},
		"beDel": false
	}],
	"styleModels": [],
	"open": true
}
```
 
