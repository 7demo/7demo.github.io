---
layout: post
title: "jquery中的promise写法"
categories:
- js
tags:
- js promise


---

日常开发中，时常遇到代码嵌套的情况，比如A完成后，再带着A的数据去执行B，伪代码可能如下：

    function A() {
        function B(aData) {
            //todo
        }
    }

这只是两层嵌套，当如是三层、四层时，就要死人了！比较那么尖的代码，总会有伤害。

而<code>promise</code>模式即是解决代码嵌套的银弹！上面的代码就可能变成这样子：

    //A , B都返回promise对象
    $.when(A())
        .then(B(aData))
        //.then(C(BData))
        //...


#ajax

其实，平常嵌套最神烦的就是ajax中的嵌套，下面就是对jquery 中ajax的稍微封装，以及应用示例：

    //para参数为字符串的时候，只为请求url，当也包含请求数据的时候，请传入obj.请求类型默认为post
    //非200的情况则都执行reject，可以触发fail事件。并把错误描述(desc)直接放入data，作为promis返回
    //请求失败的情况，把自定义的错误描述也放入data
    //并且由于jquery在1.5后的版本是直接返回的deferred对象，它是可以干预改变状态的。所以需要改为状态不可逆的promise对象
    
    var doajax = function (para) {
        
        var dtd = $.Deferred(),
            data = undefined, //请求传递参数
            type = undefined, //请求类型
            url = undefined; //请求url

        if (!para) return;

        if (typeof para === 'string') {
            url = para;
        } else {
            url = para.url;
            type = data.type;
            data = para.data;
        }
 
        $.ajax({
            type : type || 'POST',
            url : url,
            data : data,
            success : function (cbData) {
                if (cbData && typeof cbData == 'string') {
                    cbData = JSON.parse(cbData);
                }
                if (cbData.code == 200) {
                    dtd.resolve(cbData.data);
                } else {
                    dtd.reject(cbData.desc);
                }
            },
            error : function (cbData) {
                dtd.reject('网络连接错误，请检查后再次尝试');
            }
        });

        return dtd.promise();
        
    }
        

###单个请求

    doajax('http://x.xxx.com/api/url')
            .done(function (data) {
                console.log('成功', data)
            })
            .fail (function (data) {
                console.log('录失败', data)
            })
            
###多个并列请求

    $.when(doajax('http://x.xxx.com/api/url1'), doajax('http://x.xxx.com/api/url2'))
            .done(function (data, data1) {
                console.log(data, data1)
            })
            .fail(function (data) {
                console.log(data)
            })
            
            
    多个并列请求全部成功的时候，执行done,并且data根据执行的doajax顺序返回。否则触发fail。fail的data只有一个。
    
###多个串联请求<small>（上一个返回的数据作为下一个请求的必要参数）<small>

    $.when(doajax('http://t.xxx.com/api/url1'))
            .then(function (data) {
                return doajax({url : 'http://t.xxx.com/api/url2', data : data,type : 'GET'})
            }) //必须注意返回promis对象
            .then(function (data) {
                console.log(data)
            })
            .fail(function (data) {
                console.log(data)
            })
            
    执行url1的请求后，才会执行url2的执行。任何一个请求失败则执行fail。并且url1失败的情况下，不会触发url2的请求。

#一般应用

由于1.5后的jquery版本有$.Deferred对象，因为可以把一切函数改改为promise的写法

    //注意wait函数返回的是promise对象，而不是deffered对象。因为当dtd为全局时候，主动改变它的状态，即手动dtd.remove()会立即出发done方法。因此需要返回状态不可逆的promise的对象，并且dtd需为局部变量。
    
    var wait = function(){
            var dtd = $.Deferred();
　　　　　　var tasks = function(){
    　　　　　　console.log("执行完毕！");
                dtd.resolve()
    　　　　};
    　　　　setTimeout(tasks,5000);
            return dtd.promise();
    　　};
        $.when(wait())
            .done(function(){ console.log("哈哈，成功了！"); })
            .fail(function(){ console.log("出错啦！"); });
            
            
PS：<then>方法可以传两个参数，<code>sucCb</code>与<code>failCb</code>，即分别为成功与失败的回调方法。如果只有一个参数的时候，相当于<sucCb>出发<done>事件</done>。