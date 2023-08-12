<template>
    <div class="switchlist">
        <button v-for="item in takeList" v-bind:key="item" class="changeBtn">{{ item }}</button>
        <button class="addBtn changeBtn" @click="isshow=!isshow">
          <span v-if="!isshow">
            + 新增
          </span>
          <span v-else>
            - 取消
          </span>
        </button>
        <div class="newlistinput" v-if="isshow">
            <input type="text" placeholder="输入新列表名称" 
                v-model="newlistname" @keyup.enter="addNewList(this.newlistname)" />
        </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'SwitchList',
    data(){
          return{
              takeList:[
                '日常出行','列表示例2','列表示例3','列表示例4','列表示例5','列表示例6'
              ],
              isshow:false,
              newlistname:'',
          }
      },
      methods:{
        addNewList:function(e){
            if(e !== '') {
                this.takeList.push(e)
                this.newlistname = ''
                this.isshow = false
            }
            else{
                alert('列表名称为空,请输入.')
            }
        },
      }
  }
  </script>
  
  <style scoped>
  .switchlist{
    margin: 1em 0;
  }
  .changeBtn{
    margin: 0.1em 0.2em;
  }
  .addBtn{
    background-color: #c6eeff;
  }

  .newlistinput{
    margin: 0.5em 0.2em;
  }
  .newlistinput input{
    background-color: #fff;
    background-image: none;
    border-radius: 5px;
    border: 2px #cbcbcb solid;
    box-sizing: border-box;
    color: #2b2b2b;
    display: inline-block;
    font-size: inherit;
    height: 3em;
    line-height: 2em;
    outline: 0;
    padding: 0.5em 1em;
    width: 100%;
  }
  button{
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
  </style>
  