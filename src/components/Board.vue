<template>
  <div id="board" :class="classObject" v-bind:style="{backgroundColor: background||'transparent', color:color||'black'} ">
    <table class="board">
      <tr v-for="(index , row) in Board.board" track-by="$index">
        <td v-bind:style="{width: size||'60px',height: size||'60px',border: border||'3px solid black'}" v-for="(index2 , item) in row" @mouseout="mouseout()" :class="{ 'noinitial': item.noinitial }" @click="click(index,index2)" track-by="$index">{{item.value}}</td>
      </tr>
    </table>
  </div>
  <input type="text" class="hide_input">
</template>

<script>
let emptyBoard = generateArray();
//输入状态
let inputState = {
  bool: false,
  row:-1,
  col:-1,
};
//生成棋盘数组
function generateArray(char,position={row:-1,col:-1}){
  let arr = [];
  for(let i=0;i<9;i++){
  let rowArray = [];
    for(let j=0;j<9;j++){
      let c ='';
      if(i===position.row&&j===position.col){
        c=char;
      } 
      rowArray.push(c);

    }
    arr.push(rowArray);
  }
  return arr;  
}
//棋盘数组转换为Array[81]
function Array_81(arr){

  let result = [...arr];

  for(let i=0;i<9;i++){
    for(let j=0;j<9;j++){
      let item = result[i][j].value;
      if(item==''){
        result[i][j] = '%';
      }else{
        result[i][j] = item;
      }
      
    }
    result[i] = result[i].join('');
  }
  result = result.join('');
  return result;
}

///keyup事件控制器
function Keyup(e){
  if(!inputState.bool) return;
  let row = inputState.row;
  let col = inputState.col;
  let key = e.keyCode===46? '' : e.keyCode-48;
  let noinitial = this.Board.board[row][col].noinitial;

  this.Board.board[row].splice(col,1,{noinitial:noinitial,value:key});
}
//为非原始位置添加class
function addClass(obj){
  for(let i=0;i<9;i++){
    for(let j=0;j<9;j++){
      let item = obj.board[i][j];
      if(obj.noSelect){
        if(!(item<10 && item>0)){
          obj.board[i].splice(j,1,{value: item,noinitial: true});
          
        }else{
          obj.board[i].splice(j,1,{value: item,noinitial: false});
        }
      }else{
        obj.board[i].splice(j,1,{value: item,noinitial: true});
      }
    }
  }
}
/*为用户添加*/
function addUser(obj){
  for(let str in obj){
    let row = parseInt(str.slice(0,1))-1,
        col = parseInt(str.slice(1))-1;
    this.Board.board[row].splice(col,1,{noinitial: true,value:obj[str]});
  }
}
export default {
  data(){
    return {
    }
  },
  computed:{
    classObject(){
      switch(this.position){
        case 'center': return {'center': true};
        case 'top': return {'top': true};
        case 'bottom': return {'bottom': true};
        case 'left': return {'left': true};
        case 'right': return {'right': true}; 
      }
    }
    
  },
  methods:{
    click(row,col){
      if(!this.Board.board[row][col].noinitial) return;
      /*hide_input切换focus状态*/
      let hide_input = document.querySelector('.hide_input');
      hide_input.focus();
      //开启输入状态，记录位置
      inputState.bool = true;
      inputState.row = row;
      inputState.col = col;

      //添加表格样式 .focus
      let oTd = document.querySelector('.board').querySelectorAll('tr')[row].querySelectorAll('td')[col];

      oTd.classList.add('focus');

    },
    mouseout(){
      
      //删除样式
      let row = inputState.row;
      let col = inputState.col;

      let oTr = document.querySelector('.board').querySelectorAll('tr')[row];

      if(!oTr) return;

      let oTd = oTr.querySelectorAll('td')[col];
      oTd.classList.remove('focus');
      //关闭输入状态，还原位置
      inputState.bool = false;
      inputState.row = -1;
      inputState.col = -1;
      

    }
  },
  events: {
    'Array_81': function(){
      this.Board.board = Array_81(this.Board.board);
    },
    'emptyBoard': function(){
      this.Board.board = generateArray();
    }
  },
  created(){
    /*初始化数据*/
      if(this.Board){
        
        this.Board.board = (this.Board.board.length===9)? this.Board.board:generateArray();
      }else{

        this.Board = {noSelect: false,board: generateArray()};
      }
  },
  ready(){
    window.onkeyup = Keyup.bind(this);
    
    /*为非原始位置添加class*/
    addClass(this.Board);
    /*为用户添加输入的位置*/
    addUser.call(this,this.add);
  },
  props: ['Board','position','size','background','border','color','add'],
  
}
</script>

<style lang="scss">
#board {
  position: absolute;
}
.top {
  top: 0;
}
.left {
  left: 0;
}
.right {
  right: 0;
}
.bottom {
  bottom: 0;
}
.center {
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
}
.board {
  display: inline-table;
  border-collapse: collapse;
  td {
    text-align: center;
    font-weight: 600;
    
    transition: all 0.2s ease-in-out;
    &.noinitial {
      cursor: pointer;  
    }
    &.noinitial:hover {
      background-color: green;
      transform: scale(1.3);
    }
    &.noinitial.focus {
      background-color: red;
      transform: scale(1);
    }
  }

}
.hide_input {
  visibility: hidden;
}
</style>
