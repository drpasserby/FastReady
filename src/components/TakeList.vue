<template>
    <div class="takelist">
        <div class="takelistshow">
            <div class="takeitem" v-for="(item,index) in list" v-bind:key="index">
                <div class="takeitemtext">
                    {{ item }}
                </div>
                <span class="takeitembtnspan">
                    <button class="takeitembtn edit" @click="editItem(index)">
                        <svg viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"></path><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"></path></svg>
                    </button>
                    <button class="takeitembtn del" @click="delItem(index)">
                        <svg viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><polyline points="3 6 5 6 21 6"></polyline><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"></path></svg>
                    </button>
                </span>
            </div>
        </div>
        <div class="takelistadd">
            <span v-for="item in selectlist" v-bind:key="item">
                <button class="select"  @click="additem(item)">
                    {{ item }}
                </button>
            </span>
        </div>
        <div class="takelistadd">
            <input type="text" placeholder="输入物品" class="takeiteminput"
                v-model="takeAddItemInput" @keyup.enter="addInputItem()"/>
            <button class="takeitembtn add" @click="addInputItem()">
                <svg viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><line x1="12" y1="5" x2="12" y2="19"></line><line x1="5" y1="12" x2="19" y2="12"></line></svg>
            </button>
        </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'TakeList',
    data(){
          return{
            list:[
                '手机'
            ],
            selectlist:[
                '手机','钥匙','身份证','银行卡','充电宝','纸巾','眼镜','笔','U盘'
            ],
            takeAddItemInput:''
          }
      },
      methods:{
          //删除物品
          delItem:function(e){
            this.list.splice(e,1)
          },
          //编辑物品
          editItem:function(e){
            console.log(e)
          },
          addInputItem:function(){
            this.additem(this.takeAddItemInput)
            this.takeAddItemInput = ''
          },
          additem:function(e){
            if(e !== '') {
                this.list.push(e)
            }
            else{
                alert('物品名称为空,请输入.')
            }
          }
      }
  }
  </script>
  
  <style scoped>
    .takelistshow{
        width: 100%;
        border: 2px #cbcbcb solid;
        border-radius: 10px;
        padding: 0em 0.5em;
    }
    .takeitem{
        margin: 0.5em auto;
        height: 2em;
        line-height: 2em;
        background-color: #eee;
        border-radius: 9px;
        padding: 0.5em;
        transition: background-color 0.25s;
    }
    .takeitem:hover{
        background-color: #ccc;
    }
    .takeitemtext{
        float: left;
    }
    .takeitembtnspan{
        float: right;
        margin-right: 15px;
    }
    .takeitembtn{
        display: none;
        border: 0px;
        border-radius: 10px;
        color: #fff;
        transition: .1s;
        margin: 0em 0.2em;
        padding: 0.5em 1em;
        font-size: 0.5em;
    }
    .takeitem:hover .takeitembtn{
        display: inline-block;
    }
    .del{
        background-color: #f56c6c;
    }
    .del:hover{
        background-color: #ff5252;
    }
    .edit{
        background-color: #67c23a;
    }
    .edit:hover{
        background-color: #5cb860;
    }
    .select{
        border: 0px;
        border-radius: 5px;
        color: #fff;
        transition: .1s;
        margin: 0.2em 0.2em;
        padding: 0.4em 0.8em;
        font-size: 0.9em;
        line-height: 2em;
        background-color: #409eff;
    }
    .select:hover{
        background-color: #3a8ee6;
    }
    .add{
        display: inline-block;
        line-height: 2em;
        background-color: #409eff;
    }
    .add:hover{
        background-color: #3a8ee6;
    }
    .takelistadd{
        width: 100%;
        border: 2px #cbcbcb solid;
        border-radius: 10px;
        padding: 0.5em 0.5em;
        margin-top: 0.5em;
    }
    .takelistadd input{
        background-color: #fff;
        background-image: none;
        border-radius: 4px;
        border: 0px;
        box-sizing: border-box;
        color: #2b2b2b;
        display: inline-block;
        font-size: inherit;
        height: 2em;
        line-height: 2em;
        outline: 0;
        padding: 0 15px;
        width: 90%;
    }
  </style>
  