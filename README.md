# fetch.js
## 基于 es6 wepack项目运用 fetch ajax请求及跨域封装用法

### fetch.js

	//fetch.js
	import fetchJsonp from "fetch-jsonp"
	export default async(type = 'GET', url = '', data = {}, method = 'fetch') => {
	    type = type.toUpperCase();
	    if (type == 'GET') {
		let dataStr = ''; //数据拼接字符串
		Object.keys(data).forEach(key => {
		    dataStr += key + '=' + data[key] + '&';
		})

		if (dataStr !== '') {
		    dataStr = dataStr.substr(0, dataStr.lastIndexOf('&'));
		    url = url + '?' + dataStr;
		}
	    }

	    if (window.fetch && method == 'fetch') {
		let requestConfig = {
		    credentials: 'include',
		    method: type,
		    headers: {
			'Accept': 'application/json',
			'Content-Type': 'application/json'
		    },
		    mode: "cors",
		    cache: "force-cache"
		}

		if (type == 'POST') {
		    Object.defineProperty(requestConfig, 'body', {
			value: JSON.stringify(data)
		    })
		}

		try {
		    var response = await fetch(url, requestConfig);
		    var responseJson = await response.json();
		} catch (error) {
		    throw new Error(error)
		}
		return responseJson
	    } else if (fetchJsonp && method == 'fetchJsonp') {
		try {
		    var response = await fetchJsonp(url);
		    var responseJson = await response.json();
		} catch (error) {
		    throw new Error(error)
		}
		return responseJson
	    } else {
		let requestObj;
		if (window.XMLHttpRequest) {
		    requestObj = new XMLHttpRequest();
		} else {
		    requestObj = new ActiveXObject;
		}

		let sendData = '';
		if (type == 'POST') {
		    sendData = JSON.stringify(data);
		}

		requestObj.open(type, url, true);
		requestObj.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
		requestObj.send(sendData);
		requestObj.onreadystatechange = () => {
		    if (requestObj.readyState == 4) {
			if (requestObj.status == 200) {
			    let obj = requestObj.response
			    if (typeof obj !== 'object') {
				obj = JSON.parse(obj);
			    }
			    return obj
			} else {
			    throw new Error(requestObj)
			}
		    }
		}
	    }
	}

#### 1、npm install fetch-jsonp (跨域请求依赖包)


#### 2、基本请求用法
	//service.js
 	import fetch from 'fetch';
 	let token="";
 	let id="194"
     var getList = () => fetch('GET','/goods/detail', {
            token: token,
            id: id
        })



	//list.js
	import getList from 'service';
     getList(token,id).then(res => {
        console.log(res);
      }




#### 3、跨域请求用法
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
