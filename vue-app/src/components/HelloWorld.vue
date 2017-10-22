<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <ul ref='ul'>
    </ul>
  </div>
</template>

<script>
import AMap from 'AMap'
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    };
  },
  methods:{
  },
  mounted(){
  	var v=this;
  	console.log(v);
  		var mapObj = new AMap.Map('iCenter');
			mapObj.plugin('AMap.Geolocation', function() {
				var geolocation = new AMap.Geolocation({
					enableHighAccuracy: true, //是否使用高精度定位，默认:true
					timeout: 10000, //超过10秒后停止定位，默认：无穷大
					GeoLocationFirst: true, //默认为false，设置为true的时候可以调整PC端为优先使用浏览器定位，失败后使用IP定位
					maximumAge: 0, //定位结果缓存0毫秒，默认：0
					convert: true, //自动偏移坐标，偏移后的坐标为高德坐标，默认：true
					showButton: false, //显示定位按钮，默认：true
					showMarker: false, //定位成功后在定位到的位置显示点标记，默认：true
					showCircle: false, //定位成功后用圆圈表示定位精度范围，默认：true
					panToLocation: false, //定位成功后将定位到的位置作为地图中心点，默认：true
					zoomToAccuracy: false //定位成功后调整地图视野范围使定位位置及精度范围视野内可见，默认：false
				});
				mapObj.addControl(geolocation);
				geolocation.getCurrentPosition();
				AMap.event.addListener(geolocation, 'complete', onComplete); //返回定位信息
				AMap.event.addListener(geolocation, 'error', onError); //返回定位出错信息
				function onComplete(res){
					console.log(res);
					if (res) {
						var aLis='';
						var y=[];
						function getJsonMsg(json,key){
							if (typeof(json)=='object') {
								for (var key in json) {
									getJsonMsg(json[key],key);
								}
							}else{
								if (key) {
									var keyJ=key+':'+json;
									y.push(keyJ);
								} else{
									y.push(json);
								}
							}
						}
						getJsonMsg(res);
						for(var i in y){
							aLis+='<li>'+y[i]+'</li>';
						}
						v.$refs.ul.innerHTML=aLis;
					}
				}
				function onError(err) {
					console.log(err);
					if (err) {
						var aLis='';
						var y=[];
						function getJsonMsg(json,key){
							if (typeof(json)=='object') {
								for (var key in json) {
									getJsonMsg(json[key],key);
								}
							}else{
								if (key) {
									var keyJ=key+':'+json;
									y.push(keyJ);
								} else{
									y.push(json);
								}
							}
						}
						getJsonMsg(err);
						for(var i in y){
							aLis+='<li>'+y[i]+'</li>';
						}
						v.$refs.ul.innerHTML=aLis;
					}
				}
			});
  	
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
