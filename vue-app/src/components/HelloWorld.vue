<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <ul ref='ul'>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  mounted(){
  	var vue=this;
  	getLocation();
  	function getLocation(){
		  if (navigator.geolocation){ 
		    navigator.geolocation.getCurrentPosition(showPosition,showError); 
		  }else{ 
		  	
		  } 
		}
		function showPosition(position){ 
			console.log(vue);
			var aLis='';
			position.coords.latitude?aLis+='<li>纬度:' + position.coords.latitude + '</li>':'';
			position.coords.longitude?aLis+='<li>经度:' + position.coords.longitude + '</li>':'';
			aLis+='<li>定位信息:</li>';
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
			getJsonMsg(position);
			console.log(position);
			for(var i in y){
				aLis+='<li>'+y[i]+'</li>';
			}
			vue.$refs.ul.innerHTML=aLis;
		}
		function showError(error){ 
			console.log();
			switch(error.code) {
				case error.PERMISSION_DENIED:
					alert("定位失败,用户拒绝请求地理定位");
					break;
				case error.POSITION_UNAVAILABLE:
					alert("定位失败,位置信息是不可用");
					break;
				case error.TIMEOUT:
					alert("定位失败,请求获取用户位置超时");
					break;
				case error.UNKNOWN_ERROR:
					alert("定位失败,定位系统失效");
					break;
			}
			vue.$refs.ul.innerHTML='请检查浏览器定位权限为允许、手机网络和定位开启<br />Chrome、IOS10浏览器暂不支持';
		}
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
