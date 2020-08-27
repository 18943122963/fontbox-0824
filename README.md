# fontbox 长文本容器

### 介绍

传入一个范围，该容器中文字的大小将随对应的字符串长度改变，也支持使用最大长度来进行字符串截取

### 引入
```js 
import { DfaceFontbox } from 'dface-ui';
```
### 代码演示
```html
<template>
  <demo-section>
    <demo-block title="长文字容器">
      <dfaceFontbox :str="str" :config="config" />
      <dfaceFontbox :str="str2" :config="config" />
    </demo-block>
  </demo-section>
</template>

<script>
export default {
  data() {
    return {
      str: '12345',
      str2: '1234',
      config: {
        range: [
          ['min', 5, 50],
          [5, 'max', 25],
        ],
      },
    };
  },
};
</script>
```


### 配置
**str：要处理的字符串**

**config：处理配置**
- `range`: 

     处理范围，由多个有三个值的数组构成的二维数组，如[[0,100,10],[100,200,5]]，表示[0,100)长度的字符串将显示为10px大小,[100,200)长度的字符串将显示为5px大小
    
    第一个值最小长度，支持"min"，表示范围直接到达极小，
    
    第二个值最大长度，支持"max"，表示范围直接到达极大
    
    第三个值字符大小，为数字时会自动添加"px"，但也支持字符串形式，不过必须是可用的如"10px"，"2em"等，若输入了错误的字符串可能导致该该组件无效
- `max_len`: 
   
    最大长度（该组件中，字符均视为一个字），超出最大长度时不进行范围判断，将字符串截取至该长度，并字符串变为最小尺寸min_size
   
    设定该值时，优先级高于处理范围中的max
   
    不设定该值时，将自动从处理范围内取出最大长度，但此时若处理范围中存在max，则该属性失效（也就是max和max_len拿一个用就好了，max可以不进行字符串截取，max_len进行字符串截取）
- `min_size`: 
  
    不设定该值时，将自动从处理范围中取出最小尺寸


 
### 输入样例
`1 使用普通范围搭配max`
```js
divStyle = {
    str: "12345678",
    config: {
        range: [["min",5,10],[5,"max",5]]// 字符串长度在5以下（不包括5）时，字体大小将被设定为10，在5或5以上时，字体大小将被设定为5
    }
}
 输出：<div style="fontSize:5px">12345678</div>
```
 
 `2 使用max_len`
```js
divStyle = {
    str: "12345678",
    max_len: 5,
    min_size: 10,//最大长度为5，超过该长度时，截取为5并添加...，且其字体大小为10
}
 输出：<div style="fontSize:10px">12345...</div>
```
 
`3 混合使用`
```js
divStyle = {
    str: "12345678",
    config: {
    range: [["min",5,20],[5,10,15]]
    min_size: 10,//字符串长度为5以下时，字体大小为20；5-10之间为大小15；10以上截取到10，并且字体大小设为10
    }
}
 输出：<div style="fontSize:15px">12345678</div>
```