<template>
  <div class="ajax-compare">
    <el-button @click="triggerNative">触发原生封装请求</el-button>
    <el-button @click="triggerJq" type="primary">触发jQuery请求</el-button>
    <el-button @click="triggerFetch" type="success">触发fetch请求</el-button>
    <el-button @click="triggerResource" type="info">触发Vue-Resource请求</el-button>
    <el-button @click="triggerAxios" type="success">触发Axios请求</el-button>
  </div>
</template>

<script>
import Vue from 'vue'
import ajaxNative from '@/utils/ajax'
import $ from 'jquery'
import vueResource from 'vue-resource'
import axios from 'axios'
Vue.use(vueResource)
export default{
  name:'ajaxCompare',
  data(){
    return{}
  },
  mounted(){
    this.detectionPer()
  },
  methods:{
    triggerNative(){
      ajaxNative({
        url:'/api/femaleNameApi?page=1',
        type:'get',
        success:(data)=>console.log('ajax原生请求的值为',data.data)
      })
    },

    triggerJq(){
      $.get('/api/femaleNameApi', { page:1 },
      function(data){
        console.log('ajax请求值为',data.data)
      });
    },

    triggerFetch(){
      fetch("/api/femaleNameApi?page=1", {
      method: "GET"})
      .then(res =>{
        console.log('请求状态值为',res.status)
        console.log('结果状态值为',res.statusText)
        console.log('请求头信息值为',res.headers)
        console.log('请求url 值为',res.url)
        return res.json()
      }//箭头函数一行默认 return
      ).then(data=>{
        console.log('fetch请求的值为',data.data)
      })
    },

    triggerResource(){
      this.$http.get('/api/femaleNameApi', [{
        body:{page:1}
      }]).then((res) => {
        // 响应成功回调
        console.log('vueResource值为',res.data.data)
      }, (err) => {
        // 响应错误回调
      });
    },

    triggerAxios(){
      axios.get('/api/femaleNameApi', {
      params: {page:1}
    }).then(res=>{
      console.log('axios值为',res.data)
    }).catch(err=>{
      //错误返回
    })
    },

    detectionPer(){
      let timing = performance.timing,
	    readyStart = timing.fetchStart - timing.navigationStart,
	    redirectTime = timing.redirectEnd - timing.redirectStart,
	    appcacheTime = timing.domainLookupStart - timing.fetchStart,
	    unloadEventTime = timing.unloadEventEnd - timing.unloadEventStart,
	    lookupDomainTime = timing.domainLookupEnd - timing.domainLookupStart,
	    connectTime = timing.connectEnd - timing.connectStart,
	    requestTime = timing.responseEnd - timing.requestStart,
	    initDomTreeTime = timing.domInteractive - timing.responseEnd,
	    domReadyTime = timing.domComplete - timing.domInteractive,
	    loadEventTime = timing.loadEventEnd - timing.loadEventStart,
	    loadTime = timing.loadEventEnd - timing.navigationStart;
			
    console.log('准备新页面时间耗时：'+readyStart);
    console.log('redirect 重定向耗时：'+redirectTime);
    console.log('Appcache 耗时'+appcacheTime);
    console.log('unload 前文档耗时：'+unloadEventTime);
    console.log('DNS 查询耗时：'+lookupDomainTime);
    console.log('TCP 连接耗时：'+connectTime);
    console.log('request 请求耗时：'+requestTime);
    console.log('请求完毕至DOM加载：'+initDomTreeTime);
    console.log('解析DOM树耗时：'+domReadyTime);
    console.log('Load事件耗时：'+loadEventTime);
    console.log('加载时间耗时：'+loadTime);
  }

  }
}
</script>

<style lang='scss' scoped>
</style>