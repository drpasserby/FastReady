<template>
  <div class="maintitle">
    <h1>随心而遇</h1>
    <div class="link">
      <strong>To</strong>
      <a v-for="item in linkList" :key="item.name" :href="item.url" target="_blank" class="link">{{item.name}}</a>
    </div>
  </div>
  
</template>

<script>
export default {
  name: 'MainTitle',
  data(){
        return{
            linkList:[
              {name:'导航页',url:'https://wulvxinchen.cn/'},
              {name:'博客',url:'https://wulvxinchen.cn/blog'},
              {name:'工具集',url:'https://my.wulvxinchen.cn/tools/'}
            ]
        }
    },
}
</script>

<style scoped>
  .maintitle{
    width: 100%;
    border: 2px #cbcbcb solid;
    border-radius: 10px;
    padding: 2em;
  }
  .link{
    margin: 0 0.5em;
  }
</style>
