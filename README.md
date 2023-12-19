# **uniapp笔记**

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

```css
	// 单行超出显示省略号
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

