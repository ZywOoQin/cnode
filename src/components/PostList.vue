<template>
  <div class="PostList">
    <div class="loading" v-if="isloading">
      <img src="../assets/loading.gif">
    </div>
    <div class="panel" v-else>
      <ul>
        <li class="header">
          <a href="javascript:;" class="topic-tab current-tab">全部</a>
          <a href="javascript:;" class="topic-tab">精华</a>
          <a href="javascript:;" class="topic-tab">分享</a>
          <a href="javascript:;" class="topic-tab">问答</a>
          <a href="javascript:;" class="topic-tab">招聘</a>
          <a href="javascript:;" class="topic-tab">客户端测试</a>
        </li>
        <li v-for="(item,index) in items" :key="index">
          <router-link :to="{name:'userinfo_content',params:{name:item.author.loginname}}">
            <img :src="item.author.avatar_url"  :title="item.author.loginname">
          </router-link>
          <span class="reply">
            <span class="reply_count">{{item.reply_count}}</span>
            <span class="split">/</span>
            <span class="visit_count">{{item.visit_count}}</span>
          </span>
          <span :class="[{put_good:(item.good==true),put_top:(item.top==true),put_pic:(item.good!=true&&item.top!=true)}]">
            <span>{{item|tabFormation}}</span>
          </span>
          <router-link :to="{name:'article_content',params:{id:item.id,name:item.author.loginname}}">
            <span class="tit"><a href="javascript:;">{{item.title}}</a></span>
          </router-link>
          <span class="time">{{item.last_reply_at|formDate}}</span>
        </li>
      </ul>
      <div class="hh"></div>
      <PageBar @changePage="handle"></PageBar>
      <div class="hhh"></div>
    </div>
  </div>
</template>

<script>
import PageBar from './PageBar'
export default {
  name: 'PostList',
  data () {
    return {
      isloading: false,
      items:[],
      pageNum:1
    }
  },
  components:{
    PageBar
  },
  methods:{
    getData(){
      this.$http.get('https://cnodejs.org/api/v1/topics',{
        params:{
          page:this.pageNum,
          limit:25
        }
      })
      .then(res => {
        this.isloading=false;
        this.items = res.data.data
      })
      .catch(err => {
        console.log(err)
      })
    },
    handle(selectIndex){
      this.pageNum = selectIndex;
      this.getData()
    }
  },
  beforeMount:function(){
    this.isloading=true;
    this.getData()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .loading{
    position: fixed;
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%);
  }
  .loading img{
    width: 200px;
  }
  .PostList{
    background-color: #fff;
  }
  .panel{
    background-color: #fff;
    margin-bottom: 13px;
  }
  .panel .header{
    font-size: 14px;
    padding: 10px;
    background-color: #fcfcfc;
    border-radius: 3px 3px 0 0;
  }
  .topic-tab {
    margin: 0 10px;
    color: #80bd01;
  }
  .current-tab {
    background-color: #80bd01;
    color: #fff;
    padding: 3px 4px;
    border-radius: 3px;
  }
  li{
    padding: 10px;
    border: 1px #f0f0f0 solid;
    border-bottom:none;
  }
  li:first-child{
    border-top: 1px #f0f0f0 solid;
  }
  li img{
    height: 30px;
    border-radius:3px ;
    vertical-align: middle;
    display: inline-block;
  }
  li span{
    display: inline-block;
    vertical-align: middle;
  }
  .reply{
    width: 70px;
    height: 30px;
    text-align: center;
    line-height: 30px;
    font-size: 0;
  }
  .reply span{
    line-height: 30px;
    text-align: center;
    vertical-align: middle;
    line-height: 30px;
  }
  .reply_count{
    font-size: 14px;
    color: #9e78c0;
  }
  .split{
    font-size: 10px;
    color: #333;
  }
  .visit_count{
    font-size: 10px;
    color: #b4b4b4;
  }
  .tit{
    display: inline-block;
    width: 70%;
    text-overflow: ellipsis;
    overflow: hidden;
    white-space:nowrap;
  }
  .tit a{
    font-size: 16px;
    color: #333;
    text-decoration: none;
    line-height: 30px;
  }
  .tit a:hover{
    text-decoration: underline;
  }
  .time {
    float: right;
    font-size: 14px;
    line-height: 30px;
    color: #778087;
  }
  .put_good,.put_top{
    background: #80bd01;
    padding: 2px 4px;
    border-radius: 3px;
    -webkit-border-radius: 3px;
    -moz-border-radius: 3px;
    -o-border-radius: 3px;
    color: #fff;
    font-size: 12px;
  }
  .put_pic{
    background-color: #e5e5e5;
    color: #999;
    padding: 2px 4px;
    border-radius: 3px;
    -webkit-border-radius: 3px;
    -moz-border-radius: 3px;
    -o-border-radius: 3px;
    font-size: 12px;
  }
  .hh{
    height: 10px;
  }
  .hhh{
    height: 20px;
  }
</style>
