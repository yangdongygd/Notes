 // 封装请求
 import axios from 'axios'

 export function axiosRequest(url, that) {
   return axios.request({
     url: url,
     method: 'post',
     data: url,
     cancelToken: new axios.CancelToken(function executor(c) {
       that.source = c
     })
   })
 }


 // vue文件中调用
 export default {
 	data() {
 		return {
 			source: null
 		}
 	},
 	methods: {
 		someRequest() {
 			axiosRequest(url, this).then(...)

			 // 10秒后如果请求未产生结果，取消请求
			 setTimeout(() => {
				 this.source('请求被取消了')
			 }, 10000)
 		}
 	}
 }