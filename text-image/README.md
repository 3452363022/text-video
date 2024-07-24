# 多媒体文本化

![image-20220928145647380](http://mdrs.yuanjin.tech/img/202209281456408.png)

`text-image`可以将文字、图片、视频进行「文本化」

只需要通过简单的配置即可使用

源码地址：

## 开始

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Document</title>
  </head>
  <body>
    <canvas id="demo"></canvas>
    <script src="./text-image/dist/text-image.iife.js"></script>
    <script>
      textImage.createTextImage({
        canvas: document.getElementById('demo'),
        source: {
          text: 'Text Image', // 绘制的文本是：Text Image
          fontFamily: 'Roboto Mono',
        },
      });
    </script>
  </body>
</html>
```

`text-image`打包了 3 个版本的文件：

1. `text-image.iife.js`：适用于传统的引入方式，将暴露一个全局变量`textImage`，包含方法`createTextImage`

2. `text-image.js`：适用于基于 ESM 的方式导入

   ```js
   import { createTextImage } from './dist/text-image.js'
   ```

3. `text-image.umd.cjs`：适用于基于 UMD 的方式导入

## 画文字

```js
textImage.createTextImage({
  // 必填，配置canvas元素，最终作画在其上完成
  canvas: document.querySelector('canvas'),
  // 可选，配置作画的文本，默认为'6'
  replaceText: '6',
  // 可选，配置作画半径，该值越大越稀疏，默认为 10
  raduis: 10,
  // 可选，配置是否灰度化，若开启灰度化则会丢失色彩，默认为 false
  isGray: false,
  // 必填，配置作画内容
  source: {
    // 必填，配置画什么文本
    text: 'Text Image',
    // 选填，配置文本使用的字体，CSS 格式，默认为微软雅黑
    fontFamily: 'Microsoft YaHei',
    // 选填，配置文本尺寸，默认为 200
    fontSize: 200
  },
})
```

## 画图片

```js
textImage.createTextImage({
  // 必填，配置canvas元素，最终作画在其上完成
  canvas: document.querySelector('canvas'),
  // 可选，配置作画的文本，默认为'6'
  replaceText: '6',
  // 可选，配置作画半径，该值越大越稀疏，默认为 10
  raduis: 10,
  // 可选，配置是否灰度化，若开启灰度化则会丢失色彩，默认为 false
  isGray: false,
  // 必填，配置作画内容
  source: {
    // 必填，配置画的图片路径
    img: 'path',
    // 选填，配置图片宽度，默认为图片自身宽度
    width: 500,
    // 选填，配置图片高度，默认为图片自身高度
    height: 300
  },
})
```



## 画视频

```js
textImage.createTextImage({
  // 必填，配置canvas元素，最终作画在其上完成
  canvas: document.querySelector('canvas'),
  // 可选，配置作画的文本，默认为'6'
  replaceText: '6',
  // 可选，配置作画半径，该值越大越稀疏，默认为 10
  raduis: 10,
  // 可选，配置是否灰度化，若开启灰度化则会丢失色彩，默认为 false
  isGray: false,
  // 必填，配置作画内容
  source: {
    // 必填，配置画的视频路径
    video: 'path',
    // 选填，配置视频宽度，默认为视频自身宽度
    width: 500,
    // 选填，配置视频高度，默认为视频自身高度
    height: 300
  },
})
```

## text-image.iife.js

```js
var textImage=function(c){"use strict";var S=Object.defineProperty;var b=(c,n,d)=>n in c?S(c,n,{enumerable:!0,configurable:!0,writable:!0,value:d}):c[n]=d;var h=(c,n,d)=>(b(c,typeof n!="symbol"?n+"":n,d),d);class n{constructor(e,s,i){h(this,"width");h(this,"height");h(this,"pixels");this.width=e,this.height=s,this.pixels=i}getPixelAt(e,s){const i=s*this.width*4+e*4;return[this.pixels[i],this.pixels[i+1],this.pixels[i+2],+(this.pixels[i+3]/255).toFixed(2)]}}class d{constructor(){h(this,"canvas");h(this,"ctx");h(this,"isInit");this.canvas=document.createElement("canvas"),this.ctx=this.canvas.getContext("2d"),this.isInit=!1}init(){this.isInit||(this.initCanvas(),this.isInit=!0)}getBitmap(){this.init(),this.draw();const{width:e,height:s}=this.canvas,i=this.ctx.getImageData(0,0,e,s).data;return new n(e,s,i)}}class g extends d{constructor(s){super();h(this,"img");h(this,"width");h(this,"height");this.img=s.img,this.width=s.width,this.height=s.height}initCanvas(){this.canvas.width=this.width,this.canvas.height=this.height}draw(){this.ctx.drawImage(this.img,0,0,this.img.width,this.img.height,0,0,this.width,this.height)}}class x extends d{constructor(s){super();h(this,"option");this.option=s}initCanvas(){this.canvas.width=this.option.text.length*this.option.fontSize,this.canvas.height=this.option.fontSize,this.ctx.font=`bold ${this.option.fontSize}px ${this.option.fontFamily}`,this.ctx.fillStyle="#000",this.ctx.textAlign="center",this.ctx.textBaseline="middle"}draw(){this.ctx.fillText(this.option.text,this.canvas.width/2,this.canvas.height/2)}}class w extends d{constructor(s){super();h(this,"video");h(this,"width");h(this,"height");this.video=s.video,this.width=s.width,this.height=s.height,this.video.muted=this.video.loop=!0,this.video.play()}initCanvas(){this.canvas.width=this.width,this.canvas.height=this.height}draw(){this.ctx.drawImage(this.video,0,0,this.video.videoWidth,this.video.videoHeight,0,0,this.width,this.height)}}function o(t){if(t.text)return new x(t);if(t.img)return new g(t);if(t.video)return new w(t);throw new TypeError("invalid source options")}class v{constructor(e){h(this,"replaceText");h(this,"raduis");h(this,"source");h(this,"isDynamic");h(this,"canvas");h(this,"ctx");h(this,"textIndex");h(this,"isGray");h(this,"raqId");this.replaceText=e.replaceText,this.raduis=e.raduis,this.source=e.source,this.isGray=e.isGray,this.isDynamic=e.isDynamic,this.canvas=e.canvas,this.ctx=this.canvas.getContext("2d"),this.textIndex=0,this.raqId=0,this.initContext()}fps(){this.isDynamic?this.raqId=requestAnimationFrame(()=>{this.draw(),this.fps()}):this.draw()}stop(){cancelAnimationFrame(this.raqId),this.raqId=0}initContext(){this.ctx.font="bold 12px 'Roboto Mono' 'Microsoft YaHei' '\u5FAE\u8F6F\u96C5\u9ED1' 'sans-serif'",this.ctx.textAlign="center",this.ctx.textBaseline="middle"}drawText(e,s,i){let[r,a,u,l]=i;if(!l)return;this.isGray&&(r=a=u=.2126*r+.7152*a+.0722*u),this.ctx.fillStyle=`rgba(${r},${a},${u},${l})`;const T=this.replaceText[this.textIndex];this.textIndex=(this.textIndex+1)%this.replaceText.length,this.ctx.fillText(T,e,s)}draw(){const e=this.source.getBitmap();this.canvas.width=e.width,this.canvas.height=e.height,this.ctx.clearRect(0,0,this.canvas.width,this.canvas.height);for(let s=0;s<e.height;s+=this.raduis)for(let i=0;i<e.width;i+=this.raduis){const r=e.getPixelAt(i,s);this.drawText(i,s,r)}}}function f(t){if(!t)throw new Error("require options");if(!t.canvas)throw new Error('require "canvas" option');if(t.replaceText=t.replaceText||"6",t.raduis=t.raduis||10,t.isDynamic=!!t.isDynamic,t.isGray=!!t.isGray,!t.source)throw new Error('require "source" option')}function m(t){return new Promise((e,s)=>{const i=new Image;i.onload=function(){e(i)},i.onerror=function(r){s(r)},i.src=t})}function y(t){return new Promise((e,s)=>{const i=document.createElement("video");i.oncanplay=function(){e(i)},i.onerror=function(r){s(r)},i.src=t})}async function I(t){f(t);let e,s={...t};if(t.source.text)s.source=o({fontFamily:t.source.fontFamily||"Microsoft YaHei",text:t.source.text,fontSize:t.source.fontSize||200});else if(t.source.img){const i=await m(t.source.img);let r=t.source.width||i.width,a=t.source.height||i.height;t.source.width&&!t.source.height?a=r/i.width*i.height:t.source.height&&!t.source.width&&(r=a/i.height*i.width),s.source=o({img:i,width:r,height:a})}else if(t.source.video){const i=await y(t.source.video);let r=t.source.width||i.videoWidth,a=t.source.height||i.videoHeight;t.source.width&&!t.source.height?a=r/i.videoWidth*i.videoHeight:t.source.height&&!t.source.width&&(r=a/i.videoHeight*i.videoWidth),s.source=o({video:i,width:r,height:a}),s.isDynamic=!0}return e=new v(s),e.fps(),{start(){e.fps()},stop(){e.stop()}}}return c.createTextImage=I,Object.defineProperties(c,{__esModule:{value:!0},[Symbol.toStringTag]:{value:"Module"}}),c}({});
```

## text-image.js

```js
var x=Object.defineProperty;var l=(t,i,h)=>i in t?x(t,i,{enumerable:!0,configurable:!0,writable:!0,value:h}):t[i]=h;var s=(t,i,h)=>(l(t,typeof i!="symbol"?i+"":i,h),h);class g{constructor(i,h,e){s(this,"width");s(this,"height");s(this,"pixels");this.width=i,this.height=h,this.pixels=e}getPixelAt(i,h){const e=h*this.width*4+i*4;return[this.pixels[e],this.pixels[e+1],this.pixels[e+2],+(this.pixels[e+3]/255).toFixed(2)]}}class d{constructor(){s(this,"canvas");s(this,"ctx");s(this,"isInit");this.canvas=document.createElement("canvas"),this.ctx=this.canvas.getContext("2d"),this.isInit=!1}init(){this.isInit||(this.initCanvas(),this.isInit=!0)}getBitmap(){this.init(),this.draw();const{width:i,height:h}=this.canvas,e=this.ctx.getImageData(0,0,i,h).data;return new g(i,h,e)}}class w extends d{constructor(h){super();s(this,"img");s(this,"width");s(this,"height");this.img=h.img,this.width=h.width,this.height=h.height}initCanvas(){this.canvas.width=this.width,this.canvas.height=this.height}draw(){this.ctx.drawImage(this.img,0,0,this.img.width,this.img.height,0,0,this.width,this.height)}}class v extends d{constructor(h){super();s(this,"option");this.option=h}initCanvas(){this.canvas.width=this.option.text.length*this.option.fontSize,this.canvas.height=this.option.fontSize,this.ctx.font=`bold ${this.option.fontSize}px ${this.option.fontFamily}`,this.ctx.fillStyle="#000",this.ctx.textAlign="center",this.ctx.textBaseline="middle"}draw(){this.ctx.fillText(this.option.text,this.canvas.width/2,this.canvas.height/2)}}class f extends d{constructor(h){super();s(this,"video");s(this,"width");s(this,"height");this.video=h.video,this.width=h.width,this.height=h.height,this.video.muted=this.video.loop=!0,this.video.play()}initCanvas(){this.canvas.width=this.width,this.canvas.height=this.height}draw(){this.ctx.drawImage(this.video,0,0,this.video.videoWidth,this.video.videoHeight,0,0,this.width,this.height)}}function n(t){if(t.text)return new v(t);if(t.img)return new w(t);if(t.video)return new f(t);throw new TypeError("invalid source options");}class m{constructor(i){s(this,"replaceText");s(this,"raduis");s(this,"source");s(this,"isDynamic");s(this,"canvas");s(this,"ctx");s(this,"textIndex");s(this,"isGray");s(this,"raqId");this.replaceText=i.replaceText,this.raduis=i.raduis,this.source=i.source,this.isGray=i.isGray,this.isDynamic=i.isDynamic,this.canvas=i.canvas,this.ctx=this.canvas.getContext("2d"),this.textIndex=0,this.raqId=0,this.initContext()}fps(){this.isDynamic?this.raqId=requestAnimationFrame(()=>{this.draw(),this.fps()}):this.draw()}stop(){cancelAnimationFrame(this.raqId),this.raqId=0}initContext(){this.ctx.font="bold 12px 'Roboto Mono' 'Microsoft YaHei' '\u5FAE\u8F6F\u96C5\u9ED1' 'sans-serif'",this.ctx.textAlign="center",this.ctx.textBaseline="middle"}drawText(i,h,e){let[r,a,c,o]=e;if(!o)return;this.isGray&&(r=a=c=0.2126*r+0.7152*a+0.0722*c),this.ctx.fillStyle=`rgba(${r},${a},${c},${o})`;const u=this.replaceText[this.textIndex];this.textIndex=(this.textIndex+1)%this.replaceText.length,this.ctx.fillText(u,i,h)}draw(){const i=this.source.getBitmap();this.canvas.width=i.width,this.canvas.height=i.height,this.ctx.clearRect(0,0,this.canvas.width,this.canvas.height);for(let h=0;h<i.height;h+=this.raduis)for(let e=0;e<i.width;e+=this.raduis){const r=i.getPixelAt(e,h);this.drawText(e,h,r)}}}function y(t){if(!t)throw new Error("require options");if(!t.canvas)throw new Error('require "canvas" option');if(t.replaceText=t.replaceText||"6",t.raduis=t.raduis||10,t.isDynamic=!!t.isDynamic,t.isGray=!!t.isGray,!t.source)throw new Error('require "source" option');}function I(t){return new Promise((i,h)=>{const e=new Image();e.onload=function(){i(e)},e.onerror=function(r){h(r)},e.src=t})}function T(t){return new Promise((i,h)=>{const e=document.createElement("video");e.oncanplay=function(){i(e)},e.onerror=function(r){h(r)},e.src=t})}async function q(t){y(t);let i,h={...t};if(t.source.text)h.source=n({fontFamily:t.source.fontFamily||"Microsoft YaHei",text:t.source.text,fontSize:t.source.fontSize||200});else if(t.source.img){const e=await I(t.source.img);let r=t.source.width||e.width,a=t.source.height||e.height;t.source.width&&!t.source.height?a=r/e.width*e.height:t.source.height&&!t.source.width&&(r=a/e.height*e.width),h.source=n({img:e,width:r,height:a})}else if(t.source.video){const e=await T(t.source.video);let r=t.source.width||e.videoWidth,a=t.source.height||e.videoHeight;t.source.width&&!t.source.height?a=r/e.videoWidth*e.videoHeight:t.source.height&&!t.source.width&&(r=a/e.videoHeight*e.videoWidth),h.source=n({video:e,width:r,height:a}),h.isDynamic=!0}return i=new m(h),i.fps(),{start(){i.fps()},stop(){i.stop()}}}export{q as createTextImage};
```

## text-image.umd.cjs

```js
(function(c,a){typeof exports=="object"&&typeof module<"u"?a(exports):typeof define=="function"&&define.amd?define(["exports"],a):(c=typeof globalThis<"u"?globalThis:c||self,a(c.textImage={}))})(this,function(c){"use strict";var S=Object.defineProperty;var p=(c,a,d)=>a in c?S(c,a,{enumerable:!0,configurable:!0,writable:!0,value:d}):c[a]=d;var h=(c,a,d)=>(p(c,typeof a!="symbol"?a+"":a,d),d);class a{constructor(e,s,i){h(this,"width");h(this,"height");h(this,"pixels");this.width=e,this.height=s,this.pixels=i}getPixelAt(e,s){const i=s*this.width*4+e*4;return[this.pixels[i],this.pixels[i+1],this.pixels[i+2],+(this.pixels[i+3]/255).toFixed(2)]}}class d{constructor(){h(this,"canvas");h(this,"ctx");h(this,"isInit");this.canvas=document.createElement("canvas"),this.ctx=this.canvas.getContext("2d"),this.isInit=!1}init(){this.isInit||(this.initCanvas(),this.isInit=!0)}getBitmap(){this.init(),this.draw();const{width:e,height:s}=this.canvas,i=this.ctx.getImageData(0,0,e,s).data;return new a(e,s,i)}}class x extends d{constructor(s){super();h(this,"img");h(this,"width");h(this,"height");this.img=s.img,this.width=s.width,this.height=s.height}initCanvas(){this.canvas.width=this.width,this.canvas.height=this.height}draw(){this.ctx.drawImage(this.img,0,0,this.img.width,this.img.height,0,0,this.width,this.height)}}class g extends d{constructor(s){super();h(this,"option");this.option=s}initCanvas(){this.canvas.width=this.option.text.length*this.option.fontSize,this.canvas.height=this.option.fontSize,this.ctx.font=`bold ${this.option.fontSize}px ${this.option.fontFamily}`,this.ctx.fillStyle="#000",this.ctx.textAlign="center",this.ctx.textBaseline="middle"}draw(){this.ctx.fillText(this.option.text,this.canvas.width/2,this.canvas.height/2)}}class w extends d{constructor(s){super();h(this,"video");h(this,"width");h(this,"height");this.video=s.video,this.width=s.width,this.height=s.height,this.video.muted=this.video.loop=!0,this.video.play()}initCanvas(){this.canvas.width=this.width,this.canvas.height=this.height}draw(){this.ctx.drawImage(this.video,0,0,this.video.videoWidth,this.video.videoHeight,0,0,this.width,this.height)}}function o(t){if(t.text)return new g(t);if(t.img)return new x(t);if(t.video)return new w(t);throw new TypeError("invalid source options")}class f{constructor(e){h(this,"replaceText");h(this,"raduis");h(this,"source");h(this,"isDynamic");h(this,"canvas");h(this,"ctx");h(this,"textIndex");h(this,"isGray");h(this,"raqId");this.replaceText=e.replaceText,this.raduis=e.raduis,this.source=e.source,this.isGray=e.isGray,this.isDynamic=e.isDynamic,this.canvas=e.canvas,this.ctx=this.canvas.getContext("2d"),this.textIndex=0,this.raqId=0,this.initContext()}fps(){this.isDynamic?this.raqId=requestAnimationFrame(()=>{this.draw(),this.fps()}):this.draw()}stop(){cancelAnimationFrame(this.raqId),this.raqId=0}initContext(){this.ctx.font="bold 12px 'Roboto Mono' 'Microsoft YaHei' '\u5FAE\u8F6F\u96C5\u9ED1' 'sans-serif'",this.ctx.textAlign="center",this.ctx.textBaseline="middle"}drawText(e,s,i){let[r,n,u,l]=i;if(!l)return;this.isGray&&(r=n=u=.2126*r+.7152*n+.0722*u),this.ctx.fillStyle=`rgba(${r},${n},${u},${l})`;const T=this.replaceText[this.textIndex];this.textIndex=(this.textIndex+1)%this.replaceText.length,this.ctx.fillText(T,e,s)}draw(){const e=this.source.getBitmap();this.canvas.width=e.width,this.canvas.height=e.height,this.ctx.clearRect(0,0,this.canvas.width,this.canvas.height);for(let s=0;s<e.height;s+=this.raduis)for(let i=0;i<e.width;i+=this.raduis){const r=e.getPixelAt(i,s);this.drawText(i,s,r)}}}function v(t){if(!t)throw new Error("require options");if(!t.canvas)throw new Error('require "canvas" option');if(t.replaceText=t.replaceText||"6",t.raduis=t.raduis||10,t.isDynamic=!!t.isDynamic,t.isGray=!!t.isGray,!t.source)throw new Error('require "source" option')}function m(t){return new Promise((e,s)=>{const i=new Image;i.onload=function(){e(i)},i.onerror=function(r){s(r)},i.src=t})}function y(t){return new Promise((e,s)=>{const i=document.createElement("video");i.oncanplay=function(){e(i)},i.onerror=function(r){s(r)},i.src=t})}async function I(t){v(t);let e,s={...t};if(t.source.text)s.source=o({fontFamily:t.source.fontFamily||"Microsoft YaHei",text:t.source.text,fontSize:t.source.fontSize||200});else if(t.source.img){const i=await m(t.source.img);let r=t.source.width||i.width,n=t.source.height||i.height;t.source.width&&!t.source.height?n=r/i.width*i.height:t.source.height&&!t.source.width&&(r=n/i.height*i.width),s.source=o({img:i,width:r,height:n})}else if(t.source.video){const i=await y(t.source.video);let r=t.source.width||i.videoWidth,n=t.source.height||i.videoHeight;t.source.width&&!t.source.height?n=r/i.videoWidth*i.videoHeight:t.source.height&&!t.source.width&&(r=n/i.videoHeight*i.videoWidth),s.source=o({video:i,width:r,height:n}),s.isDynamic=!0}return e=new f(s),e.fps(),{start(){e.fps()},stop(){e.stop()}}}c.createTextImage=I,Object.defineProperties(c,{__esModule:{value:!0},[Symbol.toStringTag]:{value:"Module"}})});
```

