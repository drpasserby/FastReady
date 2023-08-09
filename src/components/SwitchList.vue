<template>
    <div class="switchlist">
        <button v-for="item in takeList" v-bind:key="item" class="changeBtn">{{ item }}</button>
    </div>
  </template>
  
  <script>
  export default {
    name: 'SwitchList',
    data(){
          return{
              takeList:[
                '列表示例1','列表示例2','列表示例3','列表示例4','列表示例5','列表示例6'
              ]
          }
      },
  }
  </script>
  
  <style scoped>
  .switchlist{
    margin: 2% 2%;
  }
  .changeBtn{
    margin: 0.1em 0.2em;
  }

  button {
  border-radius: 8px;
  border: 1px solid transparent;
  padding: 0.6em 1.2em;
  font-size: 1em;
  font-weight: 500;
  font-family: inherit;
  background-color: #f9f9f9;
  cursor: pointer;
  transition: background-color 0.25s;
}
button:hover {
  /* border-color: #646cff; */
  background-color: #ccc;
}
button:focus,
button:focus-visible {
  outline: 4px auto -webkit-focus-ring-color;
}
  </style>
  