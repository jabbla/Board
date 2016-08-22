<template>
  <div id="app">
    <board size="50px" :Board.sync="test" position="center"></board>
  </div>
</template>

<script>
import Board from './components/Board'

let test = [
  [1,2,'','','',2,5,8,9],
  [1,2,'','','',2,5,8,9],
  [1,2,'','','',2,5,8,9],
  [1,2,'','','',2,5,8,9],
  [1,2,'','','',2,5,8,9],
  [1,2,'','','',2,5,8,9],
  [1,2,'','','',2,5,8,9],
  [1,2,'','','',2,5,8,9],
  [1,2,'','','',2,5,8,9],
]

export default {
  data(){
    return {
      test: {
        noSelect: true,
        board: test,
      },
    }
  },
  components: {
    Board}
}
</script>

<style>
html,body{
  height: 100%;
}
#app {
  height: 100%;
}
</style>
