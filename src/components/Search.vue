<template>
  <div>
  <div class="search">
    <input  v-model="keywords" class="search-input" type="text" placeholder="在此处输入城市名字"/>
  </div>
    <div class="search-content" ref="search"  v-show="keywords">
      <ul class="search-ul">
        <li class="search-item border-bottom"
         v-for="item in list"
            :key="item.id"
        >{{item.name}}</li>
        <li class="search-item border-bottom" v-show="showWords">没有匹配的城市</li>
      </ul>
    </div>
  </div>
</template>
<script>
import Bscroll from 'better-scroll'
export default {
  props:{
    cities: Object,
    default:{}
  },

  data(){
    return{
      keywords:'',
      list:[],
      timer:null,
    }
  },
  mounted() {
    this.scroll = new  Bscroll(this.$refs.search)
  },
  updated() {
   this.scroll.refresh()
  },
  computed:{
    showWords(){
      return !this.list.length
    }
  },
  watch:{
    keywords() {
      if (!this.keywords) {
        this.list = []
        return
      }
      if (this.timer) {
        clearTimeout(this.timer)
      }
     this.timer = setTimeout(()=>{
      const result = []
       for (let i in this.cities){
        this.cities[i].forEach((value)=>{
          if (value.spell.indexOf(this.keywords) > -1 || value.name.indexOf(this.keywords) > -1){
            result.push(value)
          }
        })
       }
       this.list = result
     },100)
    }
  }
}
</script>
<style>
.search {
   position: fixed;
   overflow: hidden;
   background: #00bcd4;
   line-height: 36px;
   padding: 0  4px;
   top: 0;
   left: 0;
   width: 100%;
}
.search-input{
  box-sizing: border-box;
  height: 32px;
  line-height: 32rem;
  width: 50%;
  text-align: center;
  border-radius: 10px;
  color: #666;
  padding: 3px;
  border: none;
  outline: none;
}
.search-content{
  z-index: 1;
  position: absolute;
  overflow: hidden;
  top: 36px;
  right: 0;
  bottom: 0;
  left: 0;
  background: #cccccc
}
.search-ul {
  padding: 0 0;
  margin: 0 0;
}
.search-item{
  list-style:none;
  background: white;
  color: #666666;
  line-height: 26px;
  padding-left: 0px;
  width: 100%;
}
</style>