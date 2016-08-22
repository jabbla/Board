# Board组件需求说明
---
[Demo][1]
> props: ['Board','position','size','background','border','color','add']

## 引入
```js
import board from 'Board.vue'
```

## 样式
### 位置(默认center)
```html
<board position="left"></board>     //左侧
<board position="right"></board>    //右侧
<board position="top"></board>      //顶部
<board position="bottom"></board>   //底部
<board position="center"></board>   //中部
```
### 单元格大小
```html
<board size="80px"></board>     //每个单元格的宽高
```
### 棋盘背景色
```html
<board background="red"></board>     //设置整个棋盘的颜色为red
```
### 边框样式
```html
<board border="1px solid white"></board>     //设置整个棋盘中表格的边框样式
```
### 字体颜色
```html
<board color="white"></board>     //设置整个棋盘中字体的颜色
```

## 数据

### 棋盘初始数据
```html
<board :Board="Board"></board>
```
```js
export default {
    data:{
        Board: {
            //初始数据是否应用鼠标相关样式
            noSelect: true//true,false,默认false
            //初始棋盘数据,是9*9的数组或者是空数组
            board: [],
        }
    }
}
```
### 将数据添加到棋盘中
```html
<board :add="{'12':6}"></board>
//在第1行，第2列的位置填入6
```


  [1]: http://blog.zxrcool.com/Board