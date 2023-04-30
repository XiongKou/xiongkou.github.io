---
layout: post
title:  "axios"
categories: axios
tags: axios
author: 熊叩
---

* content
{:toc}
 
记录一下简单的axios代码











```js
import axios from 'axios'

// const baseUrl = process.env.VUE_APP_BASE_API;
const baseUrl = '';

export default {
    get: get,
    post: post,
    postImg: postImg
}

function get(url, params) {
    return send('GET', url, params);
}

function post(url, params, data) {
    return send('POST', url, params, data);
}

function postImg(url, formData) {
    return send('POST', url, {}, {}, formData);
}

function send(method, url, params, data, fd) {
    params = params || {};
    params.t = new Date().getTime();
    return axios({
        method: method,
        url: url,
        baseURL: baseUrl,
        params: params,
        data: fd ? getFormData(fd) : data ? JSON.stringify(data) : JSON.stringify({}),
        headers: {'Content-Type': fd ? 'application/x-www-form-urlencoded' : 'application/json; charset=utf-8'}
    }).then(function (res) {
        res = res.data;
        if (res.IsSuccess) {
            return res.Data;
        } else {
            window.appVue.$akdPop({message: res.Data || res.ErrorMessage});
            return Promise.reject(res);
        }
    }).catch(function (error) {
        if (error && error.response && error.response.status >= 500 && error.response.status < 600) {
            window.appVue.$akdPop({message: '服务器错误，请联系工程师处理！'});
        } else if (error && error.response && error.response.status >= 400) {
            window.appVue.$akdPop({message: '网络错误，请检查网络连接！'});
        }
        return Promise.reject(error);
    }).catch(err => {
        console.log(err);
        return Promise.reject(err);
    });
}

function getFormData(fd) {
    const formData = new FormData();
    Object.keys(fd).map(f => {
        formData.append(f, fd[f]);
    });
    return formData;
}


```