因为最近项目的需要,要做一个组织机构图展示,在网上找了很多的代码.发现都不太好,echarts是画布,很多东西不可控,而且这个部门间的连线是弯的,看着太丑,而且部门名字如果太长,基本上不会有好的展示,其它的一些jquery的插件也不太好,分的层级太多的时候,特别的丑,找了好久,终于有一个插件的效果图,让我眼前一亮: <!-- more --> 
![image](https://image-static.segmentfault.com/108/586/1085868932-5a0122d772b43_articlex)
对于组织机构来说,肯定是从上至下的关系,所以从左到右肯定不合适,而且这个插件很有意思,第二级部门为横向排列,二级后的部门为树形排列,而且左右的间距会在子节点打开的时候进行调整,我下下来看了下,发现这个部门结构图还可以进行拖动展示,所以这也比较好的避免了第二级部门太多超出页面大小的问题,毕竟能拖动的情况下,都能展示出来.  
不过这个插件中有很多其它的东西,比如部门正面显示了一些其它的东西.我看了下源码,都是后来渲染的,不过总的架构,我还是觉得很不错的.所以还是拿来用了稍微的改了下bug就好了.
具体的用法见下方代码:  

```
<title>组织机构图</title>
<div class="orgWrap">
</div>
<script type="text/javascript">
	layui.use(['admin', 'orgChart', 'jquery'], function() {
		var $ = layui.jquery;
		var admin = layui.admin;
		var orgChart = layui.orgChart;
		admin.req({
			url: "./json/jsondata.js",
			type: "get",
			success: function(res) {
				var data = res.data
				//3表示显示到部门的第3级
				orgChart.render({
					elm: '.orgWrap',
					data: data,
					drag: true,
					depth: 3,
					renderdata: function(data, $dom) {
						var value = data;
						if(value && Object.keys(value).length) {
							var $name = $('<div class="name"></div>');
							!!(value.name) && $name.text(value.name);
							$dom.append($name)
							$dom.addClass('organization')
						}
					},
					callback: function() {}
				});
			}
		});

	});
</script>
```
其中请求的org.js的json结构为:  

```
{
	"code": 0,
	"msg": "操作成功",
	"data": {
		"id": 26,
		"name": "顶级部门",
		"parentId": null,
		"children": [{
			"id": 27,
			"name": "办公室",
			"parentId": 26,
			"children": [{
				"id": 28,
				"name": "信息中心",
				"parentId": 27,
				"children": [{
					"id": 29,
					"name": "信息中心1",
					"parentId": 28
				}]
			}]
		}, {
			"id": 30,
			"name": "信息中心",
			"parentId": 26
		}, {
			"id": 31,
			"name": "采购部",
			"parentId": 26
		}, {
			"id": 44,
			"name": "部门1",
			"parentId": 26,
			"children": [{
				"id": 51,
				"name": "部门1-2",
				"parentId": 44,
				"children": [{
					"id": 52,
					"name": "部门1-2-1",
					"parentId": 51
				}]
			}, {
				"id": 53,
				"name": "部门1-1",
				"parentId": 44,
				"children": [{
					"id": 54,
					"name": "部门1-1-1",
					"parentId": 53,
					"children": [{
						"id": 55,
						"name": "部门1-1-1-1",
						"parentId": 54,
						"children": [{
							"id": 56,
							"name": "部门1-1-1-1-1",
							"parentId": 55,
							"children": [{
								"id": 57,
								"name": "部门1-1-1-1-1-1",
								"parentId": 56
							}]
						}]
					}]
				}]
			}]
		}, {
			"id": 45,
			"name": "部门2",
			"parentId": 26
		}, {
			"id": 46,
			"name": "部门3",
			"parentId": 26
		}, {
			"id": 47,
			"name": "部门4",
			"parentId": 26
		}, {
			"id": 48,
			"name": "部门5",
			"parentId": 26
		}, {
			"id": 49,
			"name": "部门5",
			"parentId": 26
		}, {
			"id": 50,
			"name": "部门6名字要长长长长长长长够长了吗?",
			"parentId": 26
		}]
	},
	"count": 20
}
```
其中,有一个部门,层级很多,大家可以看一下,另外,有的部门,名字特别长,而且二级部门很多个,也超出了页面的显示,但是可以拖动,所以也无伤大雅,总之,是可以完美展示的,见下图:  
![image](https://raw.githubusercontent.com/xieyushi/blog/master/blogimg/bloggif2.gif)
怎么样?是不是不管多少层级,多少二级部门都能完美展示啦?  
插件为两个文件:orgChart.orgChart.css,orgChart.orgChart.css放在src/style中,这个路径不要变,如果你想变,orgChart.js中去修改JS代码,不然css无法正常引入,会丢样式.  
整个插件经过lenChart.js的作者授权,同意修改变发布的,这里再推荐一下作者的博客:[刘宾的博客](https://liubin915249126.github.io/)一位前端的大牛,比我这渣渣后端,还是要强很多的.这个插件也已经发布上layui的[第三方插件库](https://fly.layui.com/extend/orgChart/),欢迎大家下载使用.同时也已经已经上传了[github](https://github.com/xieyushi/layuiadmin-yushijie)