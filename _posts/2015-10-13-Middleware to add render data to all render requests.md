---
layout: post
title: "Middleware to add render data to all render requests"
categories:
- nodejs
tags:
- nodejs


---

至少到目前为止，如题所描述的并不妥，但是最开始在`stackoverflow`上搜到了[Middleware to add render data to all render requests](http://stackoverflow.com/questions/15057857/node-js-express3-middleware-to-add-render-data-to-all-render-requests)找到要找的答案。

其实最初由于，页面中都需要用到用户名、用户id这些信息，所以在`res.render`时候变有了以下代码：

    accountRouter.get('/tutorLogin', function (req, res){
        res.render('account/login_tutor.html', {
            user: req.signedCookies, 
            nav : 'Login', 
            role : 'teacher'
        });
    });
    
    accountRouter.get('/studentLogin', function (req, res){
        res.render('account/login_student.html', {
            user: req.signedCookies, 
            nav : 'Login', 
            role : 'student'
        });
    });
    
    ...
    
太多重复的`user: req.signedCookies`，因此在想有没有比较优雅的写法，能拦截`res.render`方法，在此执行`user:req.signedCookies`,便一劳永逸的可以直接在模板上进行使用啦!

于是`stackoverflow`搜索很久，终于找到了开头提到的这个问题——`res.locals`迎刃而解啊(其实还是`API`不熟悉)！

##[res.locals][1]

>An object that contains response local variables scoped to the request, and therefore available only to the view(s) rendered during that request / response cycle (if any). Otherwise, this property is identical to app.locals.

>This property is useful for exposing request-level information such as the request path name, authenticated user, user settings, and so on.


<code>res.locals</code>是一个包含`res`本地变量的一个对象，这个对象仅对本次请求（req,res）可见，因此所有所有渲染的模板都可以用。一般用于路径名儿，权限验证，用户设置等

此外，与之相对的是`app.locals`，它的生命周期是整个应用生命周期。一般用于设置应用信息，不过在中间件内拿不到该值。

    app.locals.title = 'My App';


  [1]: http://www.expressjs.com.cn/4x/api.html#res.locals