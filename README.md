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

> - [nvue阻止事件冒泡](https://yybinb.github.io/#nvue%E9%98%BB%E6%AD%A2%E4%BA%8B%E4%BB%B6%E5%86%92%E6%B3%A1)

> - [创建原生子窗口](https://yybinb.github.io/#%E5%88%9B%E5%BB%BA%E5%8E%9F%E7%94%9F%E5%AD%90%E7%AA%97%E5%8F%A3)

> - [uniapp scrollview 不显示滚动条]() 

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

```js
//pages.json
// "app-plus": {
//     "softinputMode": "adjustResize"
// }
```

***

## nvue阻止事件冒泡

```javascript
// 标签里面使用点击事件 @click="click($event)" 传参必须添加$event
// 事件里面使用e来接收 click(e) 并在事件末尾使用 e.stopPropagation() 来阻止事件
// <view @click="click($event)"><view>
// click(e){
//     e.stopPropagation()
// }
```

***

## 创建原生子窗口

```javascript
//创建原生子窗口需要在page.json中页面配置里面创建
// "app-plus": {
//     "subNVues": [
//         {
//             "id": "instituteManagent", // 唯一标识
//             "path": "pages/index/subnvue/instituteManagent", // 页面路径
//             "style": {
//                 "position": "static",
//                 "left": "0px",
//                 "top": "0px",
//                 "background": "transparent"
//             }
//         },
//         {
//             "id": "studentJob", // 唯一标识
//             "path": "pages/index/subnvue/studentJob", // 页面路径
//             "style": {
//                 "position": "static",
//                 "left": "0px",
//                 "top": "0px",
//                 "background": "transparent"
//             }
//         }
//     ]
// }
//之后在页面中获取实例并使用事件展示与隐藏
//const subNVue = uni.getSubNVueById("instituteManagent")
//subNVue.hide(); subNVue.show();
```

***

## uniapp scrollview不显示滚动条

```css
/* #ifdef MP-WEIXIN || APP-PLUS */
		::-webkit-scrollbar {
		    display: none;
		    width: 0 !important;
		    height: 0 !important;
		    -webkit-appearance: none;
		    background: transparent;
		    color: transparent;
		  }
	/* #endif */
```

# react笔记

> - [创建项目以及配置文件](https://yybinb.github.io/#%E5%88%9B%E5%BB%BA%E9%A1%B9%E7%9B%AE%E4%BB%A5%E5%8F%8A%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)



------



## 创建项目以及配置文件

1. 创建项目：npx create-react-app 项目名称
2. 打开项目：cd 项目名称
3. 启动项目：npm start
4. 暴露配置项：npm run eject 
5. 暴露的配置项里面的path.js里面包含项目里面的配置文件及所有路径
6. index.js中引入的React负责逻辑控制，数据，VDOM。引入的ReactDOM负责渲染实际DOM->DOM

## 对象声明及渲染

​	1.字符串形式：const name = ‘App’ 

​		渲染：``` <div>{name}</div>```

 2. 对象形式：

    ```jsx
    const obj = {name:'jack',age:12}
    ```

    渲染：

    ```jsx
    <div>{nameFuction(obj)}</div>
    ```

    需要在定义方法 

    ```jsx
    nameFuction(obj){return obj.name}
    ```

    

	3. 布尔形式：

    ```jsx
    const show = true
    ```

    渲染：

    ```jsx
    <div>{ show ?  111 : 222 }</div>
    ```

    

	4. 对象形式：

    ```jsx
    const arr = [0,1,2,3,4,5,6]
    ```

    渲染：

    ```jsx
    <ul>
        {arr.map((item,index)=>{
            return <li key={index}>{item}</li>
        })}
    </ul>
    ```

## react组件

### class组件

class组件通常拥有状态和生命周期，继承于Component,实现render方法。用class组件创建一个Clock：

```jsx
import React, {Component} from "react";
// 创建class组件，继承react中的component
export default class ClassComponents extends Component {
    // 构造器
    constructor(props) {
        // 有构造器必须super，语法规定
        super(props);
        // state需要一个对象来承接
        // 这里的this指的是实例对象
        this.state = {
            name: 'Class Components',
            date: new Date().toLocaleTimeString()
        }
    }
   // 生命周期 组件挂载之后
    componentDidMount() {
        this.interval = setInterval(() => {
            // 更新状态,不能用this.state更改state中的值
            this.setState(({
                date: new Date().toLocaleTimeString()
            }))
        }, 1000)
    }
    // 生命周期 组件卸载之前执行
    componentWillUnmount() {
        clearInterval(this.interval)
    }
    render() {
        const {name, date} = this.state;

        function returnValue(value) {
            return value
        }

         return (
            <div className={'classComponents'}>
                <div>{returnValue(name)}</div>
                <div className={'classComponents_space'}></div>
                <div>{returnValue(date)}</div>
            </div>
        )
    }
}
```

### function组件

```jsx
import React,{ useState, useEffect } from "react";
import styles from '../App.module.css'

export  function FunctionComponents(props){
    const [name,setName] = useState('Function Components')

    const [date,setDate] = useState(new Date().toLocaleTimeString())

    useEffect(()=>{
        const interval = setInterval(()=>{
            setDate(new Date().toLocaleTimeString())
        },100)
        return ()=>{
            clearInterval(interval)
        }
    },[])

    return (
        <div className={styles.functionComponents}>
            <div>{name}</div>
            <div className={styles.functionComponents_space}></div>
            <div>{date}</div>
        </div>
    )
}
```

