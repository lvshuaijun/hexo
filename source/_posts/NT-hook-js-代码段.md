---
title: NT hook js 代码段
date: 2020-01-10 15:51:31
tags:
- js
- 爬虫
- 经验

header_image: /intro/papers.co-bg50-marvel-venom-logo-dark-eye-art-simple-minimal-35-3840x2160-4k-wallpaper.jpg

---

## 简介

本节主要是应用于破解逆向js过程中的hook使用方法和代码及其解释

## 使用方法

代码应用比较简单，根据不同需求代码生效时间可以设置不同
- 如果使用"油猴"插件，可以在配置选项中的：// @run-at 选项进行配置。
- 如果直接在console中使用代码段，则根据使用代码时间代码生效。

### 代码段一

hook: base64加密函数, 可以通用于别的函数调试

```js
(function() {
  function hook(object, attr) {
      var func = object[attr];
      object[attr] = function() {
        console.log('hook:', object, attr);
        var ret = func.apply(object, arguments);
        debugger;
        return ret;
      }
  }
  hook(window, 'btoa'); // btoa 就是base64，当然也可以换成别的函数名称
})();

```


### 代码段二

该代码段可以hook：eval函数和Function(函数创建会调用该函数)

```js
window.__cr_eval = window.eval;
var myeval = function(src) {
    console.log(src);
    console.log("*************** eval end *****************");
    return window.__cr_eval(src);
  
};

var _myeval = myeval.bind(null);
_myeval.toString = window.__cr_eval.toString;
Object.defineProperty(window, 'eval', {value: _myeval});

window._cr_func = window.Function;
var my_func = function() {
    var args = Array.prototype.slice.call(arguments, 0, -1).join(",");
    var src = arguments[arguments.length - 1];
    console.log(src);
    console.log("***************** Function End *******************");
    return window._cr_func.apply(this, arguments);
  
};

my_func.toString = function() {
  return window._cr_func + ""   // 当调用这个方法的时候会打印该函数的具体代码
};

Object.defineProperty(window, "Function", {value: my_func});

```

### 代码段三
websocket 的hook代码

```js
WebSocket.prototype.senda = WebSocket.prototype.send;
WebSocket.prototype.send = function(data) {
    console.log("Hook websocket", data);
    return this.senda(data);
  
};
```
### 代码段四

hook加密函数encode/encry/decode/decry（油猴插件版本）

```js
// ==UserScript==
// @name         search decode
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  !!!!
// @author       shuaijun.lv
// @include      *
// @grant        none
// ==/UserScript==
(function() {
    for (var p in window){
        var s = p.toLowerCase();
        if (s.indexOf("encode") != -1 || s.indexOf("encry") != -1){
            console.log("Detect encode function. \n", window[p]);
        }
        
        if (s.indexOf("deocde") != -1 || s.indexOf("decry") != -1){
            console.log("Detect decode function. \n", window[p]);
        }
    }
  }
)();

```

### 代码段五

该代码段可以hook到window中的一个已知属性名称的属性值GET/SET的过程！

例如：window._t, window._t.ccc

如此类比，可以继续下延！

```js
var flag_1 = '_t';
var flag_2 = 'ccc';

var key_value_map = {};
var window_value = window[flag_1];

Object.defineProperty(window, flag_1, {
    get: function() {
      console.log("Getting", window, flag_1, "=", window_value);
      //  debugger;
      return window_value;
    },
    set: function(value) {
      console.log("Setting", window, flag_1, "=", value);
      //  debugger;
      window_value = value;
      key_value_map[window[flag_1]] = flag_1;
      set_obj_attr(window[flag_1], flag_2);
    },
});
function set_obj_attr(obj, attr) {
    var obj_attr_value = obj[attr];
    Object.defineProperty(obj, attr, {
        get: function() {
            console.log("Getting", key_value_map[obj], attr, "=", obj_attr_value);
            //   debugger;
            return window_value;
        },
        set: function(value) {
            console.log("Setting", key_value_map[obj], attr, "=", value);
            //  debugger;
            obj_attr_value = value;
        },
    })
  
}
```








