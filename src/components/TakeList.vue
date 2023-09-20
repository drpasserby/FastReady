<template>
    <div class="window" v-if="isShowFloatWin != 0">
        <h3>{{ floatWinTitle }}</h3>
        <input v-model="floatWinInput" />
        <button class="mybtn export" v-if="isShowFloatWin == 1" @click="editListNameGit">确认</button>
        <button class="mybtn export" v-if="isShowFloatWin == 2" @click="addNewListGit">确认</button>
        <button class="mybtn export" v-if="isShowFloatWin == 3" @click="addNewItemGit">确认</button>
        <button class="mybtn export" @click="isShowFloatWin = 0">取消</button>
    </div>
    <div class="takelistmodule switchlist">
        <select v-model="watchWhichList">
            <option v-for="(op,index) in list.takeListList" :key="index" :value="index">
                {{ op.listTitle }}
            </option>
        </select>
        <button class="mybtn addBtn" @click="addNewList">
            <strong>＋</strong>
        </button>
    </div>
    

    <div class="takelist" v-show="shotPicData === null">
        <div class="takelistmodule takelistshow" id="TakeListShow">
            <div class="takelisttitle">📝
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
        <div class="takelistmodule">
            <div class="takelisttitle">
                🧰添加物品
            </div>
            <span v-for="item in list.selectList" v-bind:key="item">
                <button class="mybtn export"  @click="addItem(item)">
                    {{ item }}
                </button>
            </span>
            <span>
                <button class="mybtn export" @click="addNewItem()">
                    <strong>＋</strong>
                </button>
            </span>
        </div>
        <div class="takelistmodule">
            <div class="takelisttitle">
                ⚙️工具栏
            </div>
            <button class="mybtn edit" title="截图当前列表" @click="listScreenShots">
                <svg viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><path d="M23 19a2 2 0 0 1-2 2H3a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h4l2-3h6l2 3h4a2 2 0 0 1 2 2z"></path><circle cx="12" cy="13" r="4"></circle></svg>
            </button>
            <button class="mybtn edit" title="编辑当前列表名称" @click="editListName(whichList)">
                <svg viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><path d="M11 4H4a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"></path><path d="M18.5 2.5a2.121 2.121 0 0 1 3 3L12 15l-4 1 1-4 9.5-9.5z"></path></svg>
            </button>
            <button class="mybtn del" title="删除当前列表" @click="delList(whichList)">
                <svg viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><polyline points="3 6 5 6 21 6"></polyline><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"></path></svg>
            </button>
            <button class="mybtn export" title="导出所有数据" @click="exportList">
                <svg viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><polyline points="21 8 21 21 3 21 3 8"></polyline><rect x="1" y="3" width="22" height="5"></rect><line x1="10" y1="12" x2="14" y2="12"></line></svg>
            </button>
            <button class="mybtn resetting" title="重置所有数据" @click="resetList">
                <svg viewBox="0 0 24 24" width="24" height="24" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round" class="css-i6dzq1"><polyline points="17 1 21 5 17 9"></polyline><path d="M3 11V9a4 4 0 0 1 4-4h14"></path><polyline points="7 23 3 19 7 15"></polyline><path d="M21 13v2a4 4 0 0 1-4 4H3"></path></svg>
            </button>
        </div>
    </div>
    <div class="shotPicData" v-if="shotPicData != null">
        <button class="mybtn del" style="margin-bottom: 1em;" @click="closeScreenShots()">
            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-x"><line x1="18" y1="6" x2="6" y2="18"></line><line x1="6" y1="6" x2="18" y2="18"></line></svg>
        </button>
        <span><strong>&nbsp;长按保存</strong></span>
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
            isShowFloatWin: 0,
            floatWinTitle: '测试标题',
            floatWinInput: '',

            list:{},
            watchWhichList:0,
            whichList:0,
            shotPicData: null,
            isDesign:false,

            designTitle: '随心而遇 Design By LvXnCehn',
          }
      },
    methods:{
        //打开浮动窗口
        openFloatWin(e,a){
            this.floatWinInput = ''
            this.floatWinTitle = e
            this.isShowFloatWin = a
        },
        //新增列表按钮
        addNewList:function(e){
            this.openFloatWin('输入新列表名称',"2")
        },
        //新增列表提交
        addNewListGit:function(e){
            let nowtime = new Date()
            if(this.floatWinInput !== '') {
                this.list.takeListList.push({
                    listTitle:this.floatWinInput,
                    listItem:[],
                    lastTime: nowtime.toLocaleDateString() + ' ' + nowtime.toLocaleTimeString()
                })
                this.isShowFloatWin = 0
                this.updateListToLocal()
            }
            else{
                alert('列表名称为空,请输入.')
            }
        },
        //编辑列表按钮
        editListName:function(e){
            this.openFloatWin('输入列表['+this.list.takeListList[e].listTitle+']的新名称',"1")
        },
        //编辑列表提交
        editListNameGit:function(e){
            if(this.floatWinInput != ''){
                this.list.takeListList[this.whichList].listTitle = this.floatWinInput
                this.isShowFloatWin = 0
                this.updateListToLocal()
            }
            else{
                alert('列表名称为空,请输入.')
            }
        },
        //删除列表
        delList:function(e){
            if(this.list.takeListList.length ==1){
                alert('只剩下一个列表了,无法删除.')
                return 0
            }
            if(confirm('是否删除该列表?')){
                if(confirm('删除后无法恢复,是否继续?') && this.list.takeListList.length > 1){
                    this.list.takeListList.splice(e,1)
                    this.updateListToLocal()
                    //防止删除后列表为空
                    this.whichList = 0
                    this.watchWhichList = 0          
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
        //添加自定义新物品按钮
        addNewItem:function(){
            this.openFloatWin('输入自定义物品的名称',"3")
        },
        //添加自定义新物品
        addNewItemGit:function(){
            if(this.floatWinInput != ''){
                this.list.takeListList[this.whichList].listItem.push(this.floatWinInput)
                this.isShowFloatWin = 0
                this.updateListToLocal()
            }
            else{
                alert('输入为空,请输入.')
            }
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
        //重置所有列表,恢复到初始状态
        resetList:function(){
            if(confirm('是否重置所有列表?')){
                if(confirm('重置后无法恢复,是否继续?')){
                    axios.get("https://my.wulvxinchen.cn/fastready/example.json").then((response) => {
                        localStorage.setItem('TakeList',JSON.stringify(response.data))
                        window.location.reload()
                    })
                }
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
        watch:{
            watchWhichList:function(e){
                this.whichList = e
            }
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
        border-radius: 5px;
        box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.2);
    }
    .switchlist{
        display: flex;
        justify-content: center;
    }
    .switchlist select{
        width: 90%;
        height: 3em;
        padding: .4em .8em;
        border: 0;
        border-radius: 5px;
        line-height: 2em;
        font-size: 1.2em;
        font-weight: 800;
    }
    .switchlist select:focus{
        outline: none;
    }
    .takelisttitle{
        font-size: 1.3em;
        font-weight: 600;
        margin: 0.5em 0.2em;
    }
    .takelistshow{
        color: #000;
        font-weight: 400;
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
    .mybtn{
        border-radius: 5px;
        border: 1px solid transparent;
        color: #fff;
        transition: .1s;
        margin: .1em .2em;
        padding: .5em 1em;
        font-size: 1em;
        cursor: pointer;
        transition: background-color 0.25s;
    }
    .addBtn{
        color:#000;
        background-color: #eee;
    }
    .addBtn:hover{
        background-color: #ccc;
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
    .resetting{
        background-color: #e6a23c;
    }
    .resetting:hover{
        background-color: #d89f2a;
    }
    .takelistmodule{
        margin: 0.5em 0;
        padding: 0.5em 0.5em;
        border: 2px #cbcbcb solid;
        border-radius: 5px;
        background-color: #fff;
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
  