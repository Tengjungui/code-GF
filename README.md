前端代码规范
### 目录：

- JavaScript
- HTML
- CSS
- example

### JavaScript
1. 使用 === 和 !== 判等；不要使用 == 和 != 判等；
```javascript
  if(word === str)
```
2.  if(变量)足够判断变量是否为null、 '' 、undefine等情况；不用依次判断；
```javascript
  var name = getJSON().name; //这里不知道是否能取到，如果属性层次较深，建议使用getJSON()['name']
  if(name){ //简约判断
	.......
}
```
3. 能在函数作用域内声明的变量，不要放在作用域外面；
```javascript
var domain = 'xx.com'; //共享变量
var getLBSObj = function(){
	//下面的变量没有必要暴露在外面，仅供该方法使用
	var name = 'LBS',
		desc = '基于LBS的应用',
		api = ['JS', 'android', 'iOS'];
	....
}
```
4. {}的位置紧随对象和函数，例如var getName = function(){
```javascript
var getName = function(){
	var name = '大世界，小流量'；
	...
	return name;
}
```
5. js大体兼容IE8+，例如使用JSON，当然可以使用 (new Function("return " + str))();向下兼容
```javascript
var getJSONObj = function(jsonStr){
	if(JSON && JSON.parse)
		return JSON.parse(jsonStr);
	return (new Function('return' + jsonStr))();
}
```
6. 字符串全部使用单引号，例如 var str = 'id="result" name="address"';
```javascript
var div = document.createElement('div');
div.innerHTML = '<div id="news">conetnt...</div>';
```
7. 所有变量和表达式间空一格，例如for(var index in nameArr){, for(var i = 0, n = nameArr.length; i < n; i++){
```javascript
var name = 'Github';
var apiArr = ['JavaScript API', 'iOS SDK', 'Android SDK'];
// ++i或者i++ 在严格模式下，可写成i += 1;
for(var i = 0, n = apiArr.length; i < n; ++i){
	console.log(apiArr[i]);
}
```
8. 变量的命名需要见名知意，例如 var driverInfo = JSON.parse(jsonStr);
```javascript
var tipInfo = '请输入提示语'; //good case
var str1 = '请输入提示语';  // base case
```
9. 所有变量的声明在逻辑块的最前部
```javascript
var SERVER_URL = 'http://127.0.0.1:3000/amap/js'; //常量
var user = new User(); // global variable
//使用上面的全局变量
var setName = function(){
	getJSON(SERVER_URL + '/user/get');
	...
}
//一下变量被下面2个函数使用，而不被上面的函数使用，建议变量定义在逻辑块上面
//下面2个函数算是一个逻辑块
var name = 'xxxx';
var locationHash = 'nearby';
var setPath = function(id){
	location.hash = locationHash + '/' + name + '/' + id;
}
var getData = function(pid){
	getJSON(SERVER_URL + '/' + name + '/' + pid);
}
```
10.  return 后面变量不要换行，因为压缩会引起很多问题
```javascript
(function(){
	return {  //这里不要换行
		name: '测试服务',  //注意name和value之间有空格
		url: 'test:3000'
	};
})();
```
11. 建议使用 && 、 || 、！!(obj)、三元运算符等
```javascript
  if(data && data.length) //这是合理的判断，&&是依次判断，遇否中断， 建议
  var name = data.name || ''; //依次都判断，遇true赋值，建议
  var isName = names.split(',').lenght > 1 ? '名称正确'： '名称错误';
  var isCanvas = !!(canvas.getContext('2d'));
```

12. 所有callback建议return
```javascript
function getData(url, callback){
	....
	getJSON(url, function(status, data){
		if(status){
			var data = JSON.parse(data);
			data.status = status;
			return callback(data);
		}
		return callback({
			status: 0
		});
	});
}
```

13. 建议所有数组声明，使用[],例如 var arr = [];
```javascript
var names = new Array(); //不建议
var names = []; //建议
```
14. ++和--等一元自操作运算符紧跟变量
```javascript
  i++; //建议
  ++i; //建议
  i ++; //不建议
  ++ i; //不建议
```

15. 注释，建议单行注释；注释为单独一行
```javascript
//创建用户（这是单行注释，且占据一行）
var user = new User('xiaoming', 25);
//设置用户姓名
user.setName('小王');
```

16. 语句统一使用分号';'结尾，建议 var getName = function(){ ... }; //这里有分号

17. 所有的JavaScript代码放在一个闭包内,避免污染全局变量
```javascript
(function(exports){
	var user = new User();
	//do something
	...
})(window);
```

### HTML
1. 统一使用HTML5的DOCTYPE
<!DOCTYPE html>
2. 使用UTF8字符编码，全部简写成如下即可
```html
<meta charset="utf-8" />
```
3. id紧跟标签名（空一格）,即id放在标签属性的最前面
```html
<div id="" name="" ...></div>
```
4. JavaScript外部引用脚本全部放在body闭合标签上；不放在head里面
```html
<!DOCTYPE html>
<html ng-app="app">
	<head>
		<meta charset="utf-8" />
		<title></title>
	</head>
	<body>
		<div></div>
		<script type="text/javascript" src="js/angular.min.js" ></script>
		<script type="text/javascript" src="lib/1.js" ></script>
	</body>
</html>
```
5. css文件，公共样式放入common.css, 其他内联到head标签内部
```html
<head>
	<meta charset="utf-8" />
	<title></title>
	<link rel="stylesheet" href="css/common.css" />
	<style type="text/css">
		html body{...}
		body>div:nth-child(2){...}
	</style>
</head>
```
6. 建议减少使用table、H等标签，如果有必要使用display: table;建议全部使用DIV+CSS布局

7. HMTL文件中的标签属性全部使用双引号
```html
<body>
	<div id="parent">
		<div id="detail">

		</div>
	</div>
</body>
```
8. HTML标签内不建议内联样式
```html
<!--不建议内联样式-->
<div style="color:#ccc; font-size:14px;"></div>
```
9. 保持HTML文件的干净，看上去清爽，只剩下标签和class属性最好

10. scrip标签标准格式，建议不使用language
```html
<script type="text/javascript" src="外部脚本" ></script>
```

### CSS
1. 所有样式都必须分号(';')结尾, 标签和样式都在一行，不换行，减少页面代码行数
```css
html body{font-size:12px;color:#ddd;}
body>div:nth-child(2){width:300px;height:300px;border:1px solid red;}
/*初学者建议如下格式，便于代码阅读*/
html body{
  font-size:12px;
  color:#ddd;
}
```
2. 所有样式尽量使用class、标签表示；减少使用id
```css
/*一篇HTML中尽量看去只用class，必要id除外*/
.fl{float:left;}
/*尽量减少使用id*/
#title_text{float:left;}
```
3. 如果兼容低版本浏览器，建议不带前缀的css在前，然后是带前缀的样式，毕竟高版本浏览器居多
```css
div{transform: ...;-webkit-transform: ...;-moz-transform: ...;-ms-transform: ...;}
```
4. css内部属性字符串使用单引号
```css
html body{font-size:12px;color:#ddd;font-family: '微软雅黑', georgia;}
```

5. 提示信息，字体为12px; 按钮字体为14px(只针对js-demo)

6. css外部脚本标准为
```css
<link rel="stylesheet" href="css/common.css" />
```

### Example 下面是一个简单的地图显示的demo，代码如下：
```html
<!DOCTYPE HTML>
<html>
<head>
	<meta charset="utf-8">
	<title>MAP DEMO</title>
	<style type="text/css">
		body{margin:0;height:100%;width:100%;position:absolute;}
		#mapContainer{height:100%;width:100%;}
	</style>
</head>
<body>
	<div id="mapContainer"></div>

	<script type="text/javascript" src="http://webapi.amap.com/maps?v=1.3&key=8e332545f5be046f134095019aa8b5cb"></script>
	<script type="text/javascript">
		(function(AMap){

			var map = new AMap.Map('mapContainer', {
				resizeEnable: true,
				rotateEnable: true,
				dragEnable: true,
				zoomEnable: true,
				//设置可缩放的级别
				zooms: [3,18],
				//传入2D视图，设置中心点和缩放级别
				view: new AMap.View2D({
					center: new AMap.LngLat(116.397428, 39.90923),
					zoom: 13
				})
			});

			//以下为缩放逻辑代码块，地图的最小级别不能小于4
			//跟逻辑块相关的变量定义，定义在逻辑块最前部
			var MIN_ZOOM = 4;
			function setZoom(zoom){
				if(zoom >= MIN_ZOOM)
					map.setZoom(zoom);
			}

			setZoom(18);

		})(AMap);
	</script>
</body>
</html>
```
