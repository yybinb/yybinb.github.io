# **uniapp笔记**

> - [uniapp点击某个按钮跳转到当前页面的某个位置](https://yybinb.github.io/#uniapp%E7%82%B9%E5%87%BB%E6%9F%90%E4%B8%AA%E6%8C%89%E9%92%AE%E8%B7%B3%E8%BD%AC%E5%88%B0%E5%BD%93%E5%89%8D%E9%A1%B5%E9%9D%A2%E7%9A%84%E6%9F%90%E4%B8%AA%E4%BD%8D%E7%BD%AE)

> - [获取当前屏幕的高度](https://yybinb.github.io/#%E8%8E%B7%E5%8F%96%E5%BD%93%E5%89%8D%E5%B1%8F%E5%B9%95%E7%9A%84%E9%AB%98%E5%BA%A6)

> - [手机上下滑动定位到某个锚点](https://yybinb.github.io/#%E6%89%8B%E6%9C%BA%E4%B8%8A%E4%B8%8B%E6%BB%91%E5%8A%A8%E5%AE%9A%E4%BD%8D%E5%88%B0%E6%9F%90%E4%B8%AA%E9%94%9A%E7%82%B9)

> - [获取下级页面传递的参数](https://yybinb.github.io/#%E8%8E%B7%E5%8F%96%E4%B8%8B%E7%BA%A7%E9%A1%B5%E9%9D%A2%E4%BC%A0%E9%80%92%E7%9A%84%E5%8F%82%E6%95%B0)

> - [全局定义点击其他地方隐藏](https://yybinb.github.io/#%E5%85%A8%E5%B1%80%E5%AE%9A%E4%B9%89%E7%82%B9%E5%87%BB%E5%85%B6%E4%BB%96%E5%9C%B0%E6%96%B9%E9%9A%90%E8%97%8F)

> - [滑动时隐藏元素标签](https://yybinb.github.io/#%E6%BB%91%E5%8A%A8%E6%97%B6%E9%9A%90%E8%97%8F%E5%85%83%E7%B4%A0%E6%A0%87%E7%AD%BE)

> - [验证正则表达式](https://yybinb.github.io/#%E9%AA%8C%E8%AF%81%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F)

> - [获取元素的高度](https://yybinb.github.io/#%E8%8E%B7%E5%8F%96%E5%85%83%E7%B4%A0%E7%9A%84%E9%AB%98%E5%BA%A6)

> - [超出显示省略号](https://yybinb.github.io/#%E8%B6%85%E5%87%BA%E6%98%BE%E7%A4%BA%E7%9C%81%E7%95%A5%E5%8F%B7)

> - [弹出键盘时页面上推](https://yybinb.github.io/#%E5%BC%B9%E5%87%BA%E9%94%AE%E7%9B%98%E6%97%B6%E9%A1%B5%E9%9D%A2%E4%B8%8A%E6%8E%A8)

***

## **uniapp点击某个按钮跳转到当前页面的某个位置**

```javascript
uni.pageScrollTo({
	selector: `.${name}`,//你要跳转的样式位置 样式指的是id标签以及class标签
    duration: 100,//跳转时间段
})
```

***

## **获取当前屏幕的高度**

```javascript
const _this = this;
uni.getSystemInfo({
  success: res => {
    console.log('手机可用高度:' + res.windowHeight * 2 + 'rpx');
    _this.app_height = res.windowHeight * 2;
  }
});
```

***

## **手机上下滑动定位到某个锚点**

```javascript
getTop(index) {
  const query = uni.createSelectorQuery()
  query.select('.res1').boundingClientRect()
  query.select('.res2').boundingClientRect()
  query.select('.res3').boundingClientRect()
  query.select('.res4').boundingClientRect()
  query.exec((res) => {
    if (res[0].top <= 0 && res[0].bottom <= 0 && res[1].bottom > 0) {   //判断
      this.index = 1
    } else if (res[1].top <= 0 && res[1].bottom <= 0 && res[2].bottom > 0) {
      this.index = 2
    } else if (res[2].top <= 0 && res[2].bottom <= 0 && res[3].bottom > 0) {
      this.index = 3
      //如果您需要更多的页面 则这样添加即可   记得在上面获取 以及在初始化时也获取
      // else if (res[0].top == 0 && res[3].bottom <= res[0].height && res[4].bottom >= res[0].height) {
      //  this.index = 4
      // }
    } else {
      this.index = 0
    }
  })
},
```

***

## 获取下级页面传递的参数

```javascript
//上级页面点击事件监听下级页面的传参
uni.navigateTo({
  url:'/pages/institute/searchMajor',
  events:{
      //获取下级页面传递的参数
      monitorSelectedFaculties: (arr) => {
          console.log(arr)
      }
  }
})
//下级页面的传参
this.getOpenerEventChannel().emit('monitorSelectedFaculties',this.selectCollegeList)
```

***

## 全局定义点击其他地方隐藏

```javascript
Vue.directive('click-outside', {
    bind: function (el, binding, vnode) {
       el.clickOutsideEvent = function (event) {
          // 点击元素不在绑定的元素内部
          if (!(el == event.target || el.contains(event.target))) {
             vnode.context[binding.expression](event)
          }
       }
       document.body.addEventListener('click', el.clickOutsideEvent)
    },
    unbind: function (el) {
       document.body.removeEventListener('click', el.clickOutsideEvent)
    }
})

//使用v-click-outside="hide"写在元素上

//使用方法去执行
hide(index){
	if(this.addStudentShow){
    	this.addStudentShow = false
        return
    }
    if(index == 1){
        this.addStudentShow = true
   }
},
```

***

## 滑动时隐藏元素标签

```javascript
@touchmove="handletouchstart"


handletouchstart(){
   this.addStudentShow = false
},
```

***

## 验证正则表达式

```javascript
// 电话号码 ： /^(13[0-9]|14[01456879]|15[0-35-9]|16[2567]|17[0-8]|18[0-9]|19[0-35-9])\d{8}$/

// 座机号码 ： /^(0\d{2,3})-?(\d{7,8})\$

// 电子邮箱 ： /^([a-zA-Z]|[0-9])(\w|\-)+@[a-zA-Z0-9]+\.([a-zA-Z]{2,4})$/;

// 身份证号码 :

//			(1)普通校验 : /(^\d{15}$)|(^\d{18}$)|(^\d{17}(\d|X|x)$)/

//			(2)精准校验 ：

//                     18位 /^[1-9]\d{5}(19|20)\d{2}((0[1-9])|(1[0-2]))(([0-2][1-9])|10|20|30|31)\d{3}[0-9Xx]$/

//                     15位 /^[1-9]\d{5}\d{2}((0[1-9])|(1[0-2]))(([0-2][1-9])|10|20|30|31)\d{2}[0-9Xx]$/

//                     后6位 /^(([0-2][1-9])|10|20|30|31)\d{3}[0-9Xx]$/

// QQ号码 : /^[1-9][0-9]\d{4,9}$/

// 邮政编码 ： /^[1-9][0-9]\d{4,9}$/
```

***

## 获取元素的高度

```javascript
let info = uni.createSelectorQuery().select(".test_box");
info.boundingClientRect(function(data) { //data - 各种参数
    console.log(data) // 获取元素的相关信息
    if (data.height > 210) {
        that.domWidth = '730rpx'
    } else {
        that.domWidth = '520rpx'
    }
}).exec()
```

***

## 超出显示省略号

```scss
//单行超出显示省略号
overflow: hidden;
white-space: nowrap;
text-overflow: ellipsis;


//多行超出显示省略号
/* 必须结合的属性 ，将对象作为弹性伸缩盒子模型显示 。*/
display: -webkit-box;
/* 必须结合的属性 ，设置或检索伸缩盒对象的子元素的排列方式 。*/
-webkit-box-orient:vertical;
/*要显示的行数*/
-webkit-line-clamp:2;
/* 溢出部分隐藏 */
overflow:hidden;
```
***

## 弹出键盘时页面上推

```json
//pages.json
"app-plus": {
    "softinputMode": "adjustResize"
}
```

