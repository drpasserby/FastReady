<template>
    <div class="takelist" v-show="shotPicData === null">
        <div class="takelistshow" id="TakeListShow">
            <div class="takelisttitle">
                {{ list.title }}
                (共{{ list.item.length }}项)
            </div>
            <div class="takeitem" v-for="(item,index) in list.item" v-bind:key="index">
                <div class="takeitemtext">
                    {{ item }}
                </div>
                <span class="takeitembtnspan">
                    <button class="mybtn takeitembtn edit" @click="editItem(index)">
                        <svg viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"></path><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"></path></svg>
                    </button>
                    <button class="mybtn takeitembtn del" @click="delItem(index)">
                        <svg viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><polyline points="3 6 5 6 21 6"></polyline><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"></path></svg>
                    </button>
                </span>
            </div>
        </div>
        <div class="takelistadd">
            <span v-for="item in selectlist" v-bind:key="item">
                <button class="mybtn select"  @click="addItem(item)">
                    {{ item }}
                </button>
            </span>
        </div>
        <div class="takelistadd">
            <input type="text" placeholder="输入物品" class="takeiteminput"
                v-model="takeAddItemInput" @keyup.enter="addInputItem()"/>
            <button class="mybtn takeitembtn add" @click="addInputItem()">
                <svg viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><line x1="12" y1="5" x2="12" y2="19"></line><line x1="5" y1="12" x2="19" y2="12"></line></svg>
            </button>
        </div>
        <div class="takelistadd">
            <button class="mybtn edit" @click="listScreenShots">
                <svg viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><path d="M23 19a2 2 0 0 1-2 2H3a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h4l2-3h6l2 3h4a2 2 0 0 1 2 2z"></path><circle cx="12" cy="13" r="4"></circle></svg>
            </button>
            <button class="mybtn del" @click="delList()">
                <svg viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><polyline points="3 6 5 6 21 6"></polyline><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"></path></svg>
            </button>
        </div>
    </div>
    <div class="shotPicData" v-if="shotPicData != null">
        <button class="mybtn del" style="margin-bottom: 1em;"
        @click="closeScreenShots()">
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-x"><line x1="18" y1="6" x2="6" y2="18"></line><line x1="6" y1="6" x2="18" y2="18"></line></svg>
        </button>
        <br>
        <img :src="shotPicData" alt="保存截图" />
    </div>
  </template>
  
  <script>
  import html2canvas from 'html2canvas' // 引入html2canvas插件

  export default {
    name: 'TakeList',
    data(){
          return{
            list:{
                title:'日常出行',
                item:['手机'],
            },
            selectlist:[
                '手机','钥匙','身份证','银行卡','充电宝','纸巾','眼镜','笔','U盘'
            ],
            takeAddItemInput:'',
            shotPicData: null,
          }
      },
    methods:{
        //删除物品
        delItem:function(e){
            this.list.item.splice(e,1)
        },
        //编辑物品
        editItem:function(e){
            console.log(e)
        },
        addInputItem:function(){
            this.addItem(this.takeAddItemInput)
            this.takeAddItemInput = ''
        },
        addItem:function(e){
            if(e !== '') {
                this.list.item.push(e)
            }
            else{
                alert('物品名称为空,请输入.')
            }
        },
        listScreenShots:function(){
            html2canvas(document.querySelector("#TakeListShow"),
            {
                crossorigin : "anonymous",
                backgroundColor: null,
                useCORS: true, // 如果截图的内容里有图片,可能会有跨域的情况,加上这个参数,解决文件跨域问题
            }).then(canvas => {
                this.shotPicData = canvas.toDataURL('image/png')
            });
        },
        closeScreenShots:function(){
            this.shotPicData = null
        },
      }
  }
  </script>
  
  <style scoped>
    .mybtn{
        border: 0px;
        border-radius: 5px;
        color: #fff;
        transition: .1s;
        margin: 0em 0.2em;
        padding: 0.5em 1em;
        font-size: 0.5em;
    }
    .takelisttitle{
        font-size: 1.3em;
        font-weight: 600;
        margin: 0.5em 0.2em;
    }
    .takelisttitle::before{
        content: '📝';
        margin-right: 0.2em;
    }
    .takelistshow{
        /* width: 100%; */
        border: 2px #cbcbcb solid;
        border-radius: 5px;
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
        /* width: 100%; */
        border: 2px #cbcbcb solid;
        border-radius: 5px;
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
  