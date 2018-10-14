---
layout: '[layout]'
title: IEbug
date: 2018-10-14 16:53:10
tags:
---
#IE浏览器 bug 收集（未完待续。。。）
**1：请求缓存**
案例：切换账号或者项目时，部分数据还是显示之前账号的数据，没有更新
原因：IE 对于 ***相同链接相同参数的ajax请求*** 会优先读取缓存
解决：请求加上不读取缓存的方法 ，如下
>` $.ajaxSetup({cache:false})`

**2：onchange**
案例：输入框添加onchange事件反算价格，结果无效
原因：IE 对onchange方法有bug
解决：需要改成通用的onblur事件

**3：输入框自动加x**
案例：如果输入框是日期组件，会有重叠bug
原因：IE10 以上 输入框输入字符后，***会自动加上x***，导致重叠
解决：加上 去掉该样式的代码  ，如下：
>`input::-ms-clear{display:none;}`

**4：option display:none**
案例：租赁 切换option的显示和隐藏，在IE11下无效
原因：IE 对option的display:none属性无效（option不是可绘制的DOM）
解决：方法之一是用wrap方法在外层加上span标签，控制display属性 
```
$("select.js-cycle-type").find("option").each(function(i,obj){
    if($(obj).parent(".parentspan").length>0){
        $(obj).unwrap();
    }
    if($(obj).css("display")=='none'){
        $(obj).wrap($("<span class='parentspan' style='display:none;'></span>"));
    }
});
```