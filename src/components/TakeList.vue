<template>
    <div class="window" v-if="isShowFloatWin">
        <h2>{{ floatWinTitle }}</h2>
        <input v-model="floatWinInput" />
        <button @click="getFloatWinInput">确认</button>
    </div>
    <div class="switchlist">
        <button v-for="(item,index) in list.takeListList" v-bind:key="index" class="changeBtn" 
        @click="changeTo(index)" @dblclick="editListName(index)">
          {{ item.listTitle }}
      </button>
        <button class="addBtn changeBtn" @click="isNewShow=!isNewShow">
          <span v-if="!isNewShow">
            + 新增
          </span>
          <span v-else>
            - 取消
          </span>
        </button>
        <div class="newlistinput" v-if="isNewShow">
            <input type="text" placeholder="输入新列表名称" 
                v-model="newlistname" @keyup.enter="addNewList(this.newlistname)" />
        </div>
    </div>

    <div class="takelist" v-show="shotPicData === null">
        <div class="takelistshow" id="TakeListShow">
            <div class="takelisttitle">
                {{ list.takeListList[whichList].listTitle }}
                (共{{ list.takeListList[whichList].listItem.length }}项)
            </div>
            <div class="takeitem" v-for="(item,index) in list.takeListList[whichList].listItem" v-bind:key="index">
                <div class="takeitemtext">
                    {{ item }}
                </div>
                <span class="takeitembtnspan">
                    <button class="mybtn edit" @click="editItem(index)">
                        <svg viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"></path><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"></path></svg>
                    </button>
                    <button class="mybtn del" @click="delItem(index)">
                        <svg viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><polyline points="3 6 5 6 21 6"></polyline><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"></path></svg>
                    </button>
                </span>
            </div>
            <div class="design" v-show="isDesign">
                {{ designTitle }}
            </div>
        </div>
        <div class="takelistadd">
            <span v-for="item in list.selectList" v-bind:key="item">
                <button class="mybtn select"  @click="addItem(item)">
                    {{ item }}
                </button>
            </span>
        </div>
        <div class="takelistadd">
            <input type="text" placeholder="输入物品" class="takeiteminput"
                v-model="takeAddItemInput" @keyup.enter="addInputItem()"/>
            <button class="mybtn add" @click="addInputItem()">
                <svg viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><line x1="12" y1="5" x2="12" y2="19"></line><line x1="5" y1="12" x2="19" y2="12"></line></svg>
            </button>
        </div>
        <div class="takelistadd">
            <button class="mybtn edit" @click="listScreenShots">
                <svg viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><path d="M23 19a2 2 0 0 1-2 2H3a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h4l2-3h6l2 3h4a2 2 0 0 1 2 2z"></path><circle cx="12" cy="13" r="4"></circle></svg>
            </button>
            <button class="mybtn edit" @click="editList(whichList)">
                <svg viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"></path><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"></path></svg>
            </button>
            <button class="mybtn del" @click="delList(whichList)">
                <svg viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><polyline points="3 6 5 6 21 6"></polyline><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"></path></svg>
            </button>
            <button class="mybtn export" @click="exportList()">
                <svg viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><polyline points="21 8 21 21 3 21 3 8"></polyline><rect x="1" y="3" width="22" height="5"></rect><line x1="10" y1="12" x2="14" y2="12"></line></svg>
            </button>
        </div>
    </div>
    <div class="shotPicData" v-if="shotPicData != null">
        <button class="mybtn del" style="margin-bottom: 1em;" @click="closeScreenShots()">
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-x"><line x1="18" y1="6" x2="6" y2="18"></line><line x1="6" y1="6" x2="18" y2="18"></line></svg>
        </button>
        <br>
        <img :src="shotPicData" alt="保存截图" class="showPicClass"/>
    </div>
  </template>
  
  <script>
  import html2canvas from 'html2canvas' // 引入html2canvas插件
  import axios from 'axios' // 引入axios插件

  export default {
    name: 'TakeList',
    data(){
          return{
            //浮窗输入框设置
            isShowFloatWin: false,
            floatWinTitle: '测试标题',
            floatWinInput: '',

            list:{},
            whichList:0,
            newlistname:'',
            takeAddItemInput:'',
            shotPicData: null,
            isDesign:false,
            isNewShow:false,
            // isEditShow:-1,
            designTitle: '随心而遇 Design By LvXnCehn',
          }
      },
    methods:{
        openFloatWin(e){
            this.floatWinTitle = e
            this.isShowFloatWin = true
        },
        getFloatWinInput(e){
            console.log(e)
            this.floatWinInput = e
            this.isShowFloatWin = false
            return this.floatWinInput
        },
        //新增列表
        addNewList:function(e){
            let nowtime = new Date()
            if(e !== '') {
                this.list.takeListList.push({
                    listTitle:e,
                    listItem:[],
                    lastTime: nowtime.toLocaleDateString() + ' ' + nowtime.toLocaleTimeString()
                })
                this.newlistname = ''
                this.isNewShow = false
                this.updateListToLocal()
            }
            else{
                alert('列表名称为空,请输入.')
            }
        },
        //双击编辑列表
        editListName:function(e){
            this.isEditShow = e
            this.newlistname = this.list.takeListList[e].listTitle
        },
        //编辑列表提交
        // editListNameGit:function(e){
        //     this.list.takeListList[this.isEditShow].listTitle = this.newlistname
        //     this.newlistname = ''
        //     this.isEditShow = -1
        //     this.updateListToLocal()
        // },
        //删除列表
        delList:function(e){
            if(confirm('是否删除该列表?')){
                if(confirm('删除后无法恢复,是否继续?') && this.list.takeListList.length > 1){
                    this.list.takeListList.splice(e,1)
                    this.updateListToLocal()
                }
            }
        },
        //删除物品
        delItem:function(e){
            this.list.takeListList[this.whichList].listItem.splice(e,1)
            this.updateListToLocal()
        },
        //编辑物品
        editItem:function(e){
            console.log(e)
        },
        //
        addInputItem:function(){
            this.addItem(this.takeAddItemInput)
            this.takeAddItemInput = ''
        },
        //添加物品
        addItem:function(e){
            if(e !== '') {
                this.list.takeListList[this.whichList].listItem.push(e)
                this.updateListToLocal()
            }
            else{
                alert('物品名称为空,请输入.')
            }
        },
        //截图列表
        listScreenShots:function(){
            this.isDesign = true
            setTimeout(() => {
                html2canvas(document.querySelector("#TakeListShow"),
                {
                    crossorigin : "anonymous",
                    backgroundColor: null,
                    useCORS: true, // 如果截图的内容里有图片,可能会有跨域的情况,加上这个参数,解决文件跨域问题
                }).then(canvas => {
                    this.shotPicData = canvas.toDataURL('image/png')
                });
            }, 5);
        },
        //关闭截图
        closeScreenShots:function(){
            this.isDesign = false
            this.shotPicData = null
        },
        //切换列表
        changeTo:function(e){
            this.whichList = e
        },
        //导出json文件
        exportList:function(){
            if(confirm('是否导出数据为JSON文件?')){
                var data=this.list//处理得到的json字符串
                var filename = 'export.json'//json文件名
                if(typeof data === 'object'){
                    data = JSON.stringify(data, undefined, 4)
                }
                var blob = new Blob([data], {type: 'text/json'}),
                e = document.createEvent('MouseEvents'),
                a = document.createElement('a')
                a.download = filename
                a.href = window.URL.createObjectURL(blob)
                a.dataset.downloadurl = ['text/json', a.download, a.href].join(':')
                e.initMouseEvent('click', true, false, window, 0, 0, 0, 0, 0, false, false, false, false, 0, null)
                a.dispatchEvent(e)
            }
        },
        //检查列表完整性
        checkList:function(){
            if(this.list.selectList != null && this.list.takeListList != null){
                console.log('选择列表或者清单列表正确')
                return true
            }
            else{
                alert('选择列表或者清单列表错误，数据出错.')
                return false
            }
        },
        //更新列表到本地
        updateListToLocal:function(){
            localStorage.setItem('TakeList',JSON.stringify(this.list))
        },
        //读取或创建列表
        readOrCreateList:function(){
            if(localStorage.getItem('TakeList') != null){
              this.list = JSON.parse(localStorage.getItem('TakeList'))
            }
            else{
                axios.get("https://my.wulvxinchen.cn/fastready/example.json").then((response) => {
                    localStorage.setItem('TakeList',JSON.stringify(response.data))
                    window.location.reload()
                })
            }
        }
      },
        beforeMount(){
            this.readOrCreateList()
        },
  }
  </script>
  
  <style scoped>
    .window{
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: white;
            padding: 20px;
            border: 1px solid #ccc;
            box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.2);
    }
    .window button{
        margin: 0 .5em;
        padding: .3em .8em;
        font-weight: 500;
        color: #fff;
        background-color: #409eff;
    }
    .window button:hover {
        background-color: #3a8ee6;
    }
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
    .mybtn{
        color: #000;
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
        background-color: #fff;
        border: 2px #cbcbcb solid;
        color: #000;
        font-weight: 400;
        border-radius: 5px;
        padding: 0em 0.5em;
    }
    .takeitem{
        margin: 0.5em auto;
        line-height: 2em;
        background-color: #eee;
        border-radius: 9px;
        padding: 0.5em;
        transition: background-color 0.25s;
    }
    .takeitem:hover{
        background-color: #ccc;
    }
    .takeitem:hover .takeitemtext{
        font-weight: 800;
    }
    .takeitembtnspan{
        display: none;
    }
    .takeitem:hover .takeitembtnspan{
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
    .export{
        background-color: #409eff;
    }
    .export:hover{
        background-color: #3a8ee6;
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
    .design{
        margin: 0.5em 0.2em;
        font-size: 0.6em;
        color: #bbb;
    }
    .showPicClass{
        max-width: 100%;
        max-height: 100%;
    }
  </style>
  