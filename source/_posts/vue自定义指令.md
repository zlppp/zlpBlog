---
title: 【实用】整理常用的vue自定义指令
date: 2020-12-31 16:29:44
tags: 
---
## 自定义指令参数详解：
+  bind：只调用一次，指令第一次绑定到元素时调用，可以定义一个在绑定时执行一次的初始化动作
-  inserted：被绑定元素插入父节点时调用
-  update：被绑定元素所在的模板更新时调用，但是可能发生在其子 VNode 更新之前
-  componentUpdated：指令所在组件的 VNode 及其子 VNode 全部更新后调用
-  unbind：只调用一次，指令与元素解绑时调用

## 自定义指令
- v-permission 按钮权限
- v-waterMark 图片水印
- v-copy 复制文本
- v-table-scroll 表格滑动
- v-debounce 文本框防抖

## v-focus 元素获得焦点
我们先来看一个vue官网的例子
<!-- <img src="/zlpBlog/images/directive/image.png"  /> -->

```html
<input v-focus placeholder="自动获取焦点" />
```
```javascript
// 注册一个全局自定义指令 `v-focus`
Vue.directive('focus', {
  // 当被绑定的元素插入到 DOM 中时……
  inserted: function (el) { // 聚焦元素
    el.focus()
  }
})
```


## v-permission 按钮权限
背景：在管理后台中，经常需要根据登录的角色来控制按钮操作权限
需求：自定义一个指令，对需要用权限来控制的DOM，显示或隐藏DOM

```html
<button v-has="'edit'">修改</button>
```
```javascript
Vue.directive('has', {
  inserted: (el, binding) => {
    // 获取登录用户权限列表
    let funList = store.state.permission.funList || []
    if(!funList.includes(binding.value)) {
      // 没有权限则删除节点
      el.parentNode.removeChild(el)
    }
  }
})
```

## v-waterMark 图片水印
<img src="/zlpBlog/images/directive/image.png"  />

需求：给特定的图片添加水印，如身份证正反面，营业资质等
思路：1、先设置一块字体旋转45度的正方形水印；2、将水印平铺满图片

```javascript
<img v-watermark="'我是水印'" src="@/assets/fubei.png">
```
```javascript
function watermarkText (text) {
  let watercanvas = document.createElement('canvas')
  let trx = watercanvas.getContext('2d')
  // 根据文字长度设置画布长宽
  let textLen = text.length * 20
  watercanvas.width = textLen
  watercanvas.height = textLen
  // 找出中心点
  let cx = textLen / 2, cy = textLen / 2
  //坐标圆点设置到形状中心
  trx.translate(cx, cy)            
  // 逆时针旋转 45 度
  trx.rotate((Math.PI / 180) * -45)
  trx.translate(-cx, -cy)
  // 设置字体、颜色
  trx.font = '18px 黑体'
  trx.fillStyle = "rgba(255,255,255,.4)"
  trx.fillText(text, 0, textLen / 2)
  return watercanvas
}

Vue.directive('watermark', {
  inserted: (el, binding) => {
    let imgUrl = el.src
    let mycanvas =  document.createElement('canvas')
    let crx = mycanvas.getContext('2d')
    let img = new Image()
    img.src = imgUrl
    img.onload = function () {
      mycanvas.width = el.width
      mycanvas.height = el.height
      crx.drawImage(img, 0, 0,el.width,el.height)
      // 水印平铺图片
      crx.fillStyle = crx.createPattern(watermarkText(binding.value.text), 'repeat');
      crx.fillRect(0, 0, el.width, el.height)
    }
    // 替换原本图片
    el.parentNode.replaceChild(mycanvas, el)
  }
})
```

## v-copy 复制文本
<img src="/zlpBlog/images/directive/11.gif" style="border:none;" />
需求：实现用户一键复制文本内容，用于用户右键粘贴文本

```javascript
<button v-copy="'需要复制的文本'">复制</button>
```
```javascript
// 这里使用的是clipboard，先安装clipboard
npm i clipboard --save
```

```javascript
// 引入clipboard
import Clipboard from 'clipboard'

Vue.directive('copy', {
  inserted: (el, binding) => {
    let clipboard = new Clipboard(el, { text: () => binding.value })
    clipboard.on('success', e => {
      console.log('复制成功！')
      clipboard.destroy()
    })
    clipboard.on('error', e => {
       console.log('复制失败，请手动复制')
       clipboard.destroy()
    })
  }
})
```

## v-table-scroll 表格滑动
<img src="/zlpBlog/images/directive/1111.gif" style="border:none;" />
需求：当表格内容过长，可以直接滑动表格，不用拉动下面的滚动条，使用户操作更简便

```javascript
Vue.directive('table-scroll', {
  inserted: (el, binding) => {
    let drag = function (obj) {
      let gapX = 0
      let startx = 0
      obj.addEventListener('mousedown', start, false)
      function start (event) {
        event.preventDefault()
        event.stopPropagation()
        if (event.button === 0) { // 判断是否点击鼠标左键
          gapX = event.clientX
          startx = obj.scrollLeft // scroll的初始位置
          document.addEventListener('mousemove', move, false)
          document.addEventListener('mouseup', stop, false)
        }
      }
      function move (event) {
        event.preventDefault()
        event.stopPropagation()
        let left = event.clientX - gapX // 鼠标移动的相对距离
        obj.scrollLeft = startx - left
      }
      function stop () {
        document.removeEventListener('mousemove', move, false)
        document.removeEventListener('mouseup', stop, false)
      }
    }
    const scrollElm = el.querySelector('.ant-table-scroll .ant-table-body')
    if (scrollElm) {
      scrollElm.style.cursor = 'move'
      scrollElm.style.userSelect = 'none'
      drag(scrollElm)
    }
  }
})
```

## v-debounce 文本框防抖
<img src="/zlpBlog/images/directive/debounce.gif" style="border:none;" />

防抖：在持续触发事件时，一定时间段内只执行一次
需求：在文本框输入时，触发事件时请求后台搜索数据且不影响性能

```javascript
<a-input v-debounce="{fn: inputKeyUp, delay: 500}" v-model="value" placeholder="防抖的文本框"></a-input>

methods: {
    inputKeyUp () {
      if (this.value) {
        console.log(this.value)
      	// 请求后台数据
      }
    }
}
```
```javascript
Vue.directive('debounce', {
  inserted: (el, binding) => {
    let { fn, delay } = binding.value
    let timer = null
    el.addEventListener('keyup', function() { // 给文本框添加keyup事件
      if (timer) { clearTimeout(timer) }
      timer = setTimeout(() => {
        fn.apply(this, arguments)
        timer = null
      }, delay)
    })
  }
})
```
