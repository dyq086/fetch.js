# fetch.js
基于fetch ajax请求及跨域封装



基本用法：

基于es6 wepack项目运用

1、npm install fetch-jsonp (跨域请求依赖包)


2、基本请求用法
	//service.js
 	import fetch from 'fetch';
 	let token="";
 	let id="194"
     var getList = () => fetch('GET','/goods/detail', {
            token: token,
            id: id
        })
	  export{getList}


	//list.js
	import getList from 'service';
     getList(token,id).then(res => {
        console.log(res);
      }




  3、跨域请求用法
	//service.js
 	import fetch from 'fetch';
 	let token="";
 	let id="194"
     var getList = () => fetch('GET','https://api-mall.xxxx.com/goods/detail', {
            token: token,
            id: id
        },'fetchJsonp')
	  export{getList}


	//list.js
	import getList from 'service';
     getList(token,id).then(res => {
        console.log(res);
      }



   参考:http://www.tuicool.com/articles/7fA3iu