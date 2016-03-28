---
layout: post
title: "video播放器，兼容IE8"
categories:
- html5
tags:
- html5


---

今天涉及到音视频播放，因为要兼容ie8，所以尝试了很多第三方代码库<code>video.js</code>, <code>jwplay.js</code>, <code>html5video.js</code>等等。但是在跑demo的时候，大都跪在IE8上。

后来google后，找到一个[解决方案][1]：

`<video controls="controls" poster="http://www.zm1v1.com/static/desktop/static/popular-teacher/img/img1_e815144.png" width="640" height="360">
    <source src="http://7xpbjt.media1.z0.glb.clouddn.com/video11.mp4" type="video/mp4" />
    <object type="application/x-shockwave-flash" data="http://flashfox.googlecode.com/svn/trunk/flashfox.swf" width="640" height="360">
        <param name="movie" value="http://flashfox.googlecode.com/svn/trunk/flashfox.swf" />
        <param name="allowFullScreen" value="true" />
        <param name="wmode" value="transparent" />
        <param name="flashVars" value="controls=true&amp;poster=http://www.zm1v1.com/static/desktop/static/popular-teacher/img/img1_e815144.png&amp;src=http://7xpbjt.media1.z0.glb.clouddn.com/video11.mp4" />
        <img alt="Big Buck Bunny" src="http://www.zm1v1.com/static/desktop/static/popular-teacher/img/img1_e815144.png" width="640" height="360" title="No video playback capabilities, please download the video below" />
    </object>
</video>`

不需要任何额外代码。

不过需要注意的是，播放的音视频文件必须在服务器上，mp4格式的视频应为为H264.


 [1]: https://www.dev-metal.com/crossbrowser-safe-html5-video-ie6-lines-code/
