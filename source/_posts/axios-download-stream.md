---
title: Web 端Axios 下载文件流
date: 2021-12-17 19:00:09
tags: [JavaScript,js,Web,stream,下载,download,Axios]
categories: 饭碗(技术)
---

web 端 利用Axios 网络库，来下载文件流。如：下载后端返回的execl文件流。

### 封装

利用axios 的 拦截器 来处理接口返回的结果。创建Blob 对象的url, 手动出发click 时间下载文件。

```js
// request.js

import axios from 'axios'

const download = axios.create({
  method: 'GET',
  withCredentials: true,
  baseURL: '/api',
  responseType: 'blob',
  exposedHeaders: ['Content-Disposition']
})

download.interceptors.request.use(config => {
  const token = Vue.ls.get(ACCESS_TOKEN)
  if (token) {
    config.headers['Authorization'] = `Bearer ${token}` // 让每个请求携带自定义 token 请根据实际情况自行修改
  }
  return config
}, err)

download.interceptors.response.use((response) => {
  const { config } = response
  const blob = new Blob([response.data], { type: response.headers['content-type'] })
  const url = window.URL.createObjectURL(blob)
  const link = document.createElement('a')
  link.href = url
  const fileName = response.request.getResponseHeader('Content-Disposition')?.match(/(filename=)(.*)(?=;)/g)?.[0]?.split('=')[1]

  link.setAttribute('download', fileName ? decodeURIComponent(fileName) : config.downloadFileName)
  link.click()
  window.URL.revokeObjectURL(url)
  return response
})

export { axiosDownload }
```

### 使用

```js
// api.js

import { axiosDownload } from './request.js'

export function exportData (parameter) {
  return axiosDownload({
    url: '/order/export',
    params: parameter
  })
}
```
```vue
<template>
  <button :loading="loading" @click="export">导出Execl</button>
</template>
<script>
import { exportData } from './api.js'

export default {
  data() {
    return {
      loading: false,
    }
  },
  methods: {
    export() {
      const payload = {}
      exportData(payload).then(res => {
          this.loading = false
          return true
        })
        .catch(res => {
          console.log('导出报错: ', res)
          this.loading = false
          this.$message.error('导出失败')
          return false
        })
    }
  }
}
</script>
```
