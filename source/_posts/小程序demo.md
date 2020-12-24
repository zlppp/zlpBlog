---
title: 模拟滴滴旧版本小程序
date: 2020-06-22 19:58:11
tags:
---

> 项目地址：https://github.com/zlppp/didiWetChatMina

## 小程序截图预览

<div style="display: flex;justify-content: flex-start">
  <img src="/zlpBlog/images/didiTaxi/img1.jpg"  style="width: 300px;margin-left: 10px;" />
  <img src="/zlpBlog/images/didiTaxi/img2.jpg"  style="width: 300px;margin-left: 10px;" />
</div>
<div style="display: flex;justify-content: flex-start">
  <img src="/zlpBlog/images/didiTaxi/img3.jpg"  style="width: 300px;margin-left: 10px;" />
  <img src="/zlpBlog/images/didiTaxi/img4.jpg"  style="width: 300px;margin-left: 10px;" />
</div>
<div style="display: flex;justify-content: flex-start">
  <img src="/zlpBlog/images/didiTaxi/img5.jpg"  style="width: 300px;margin-left: 10px;" />
  <img src="/zlpBlog/images/didiTaxi/img6.jpg"  style="width: 300px;margin-left: 10px;" />
</div>
<div style="display: flex;justify-content: flex-start">
  <img src="/zlpBlog/images/didiTaxi/img7.jpg"  style="width: 300px;margin-left: 10px;" />
</div>

## 代码片段
根据城市首字母排序，并跳到相应城市
```javascript
  
  /**
   * 组件的初始数据
   */
  data: {
    copyCityList: [], // 城市列表副本 
    sortCityList: [], // 根据字母排序后的城市列表
    scrollTopId: '',
    scrollHeight: ''
  },
  ready () {
    let cityList = this.data.cityList // 完整的城里列表
    for(let i  = 0; i < this.data.cityList.length; i++) {
      // 取出首字母
      let firstLetter = cityList[i].pinyin[0].slice(0,1).toUpperCase()
      if (this.isCityList(firstLetter)) {
        for (var k = 0; k < this.data.copyCityList.length; k++) {
          if (firstLetter === this.data.copyCityList[k].index) {
            this.data.copyCityList[k].list.push({
              id: cityList[i].id,
              name: cityList[i].name,
              fullName: cityList[i].fullname,
              location: cityList[i].location
            })
            break;
          }
        }
      } else {
        this.data.copyCityList.push({
          index: firstLetter,
          list: [{
            id: cityList[i].id,
            name: cityList[i].name,
            fullName: cityList[i].fullname,
            location: cityList[i].location
          }]
        })
      }
    }
    this.sortLetter()
    this.setData({
      sortCityList: this.data.copyCityList
    })
    this.getSrollHeight()
  },
  /**
   * 组件的方法列表
   */
  methods: {
    /**
     * 设置scroll-view高度
     */
    getSrollHeight () {
      let windowHeight = app.globalData.systemInfoSync.windowHeight // 屏幕的高度
      this.setData({
        scrollHeight: windowHeight * 750 - 100
      })
    },
    /**
     * @function 是否已存在首字母
     **/
    isCityList (firstLetter) {
      var bStop = false
      for (var i = 0; i < this.data.copyCityList.length; i++) {
        if (this.data.copyCityList[i].index == firstLetter) {
          bStop = true
          break
        }
      }
      return bStop //存在为true 不存在未false
    },
    /**
     * @function 排序
     **/
    sortLetter () {
      this.data.copyCityList.sort((item1, item2) => {
        if (item1.index > item2.index) {
          return 1;
        } else {
          return -1;
        }
      })
    },
    /**
     * @function 选择的城市
     */
    handCity (e) {
      this.triggerEvent('confirm', { item: e.currentTarget.dataset.item })
    },
    /**
     * @function 跳转到对应字母，将首字母赋值
     */
    selecteChooseHandle(e) {
      this.setData({
        scrollTopId: e.target.id
      })
    }
  }
})
```
