<template>
  <div>
    <div class="loading" v-if="isloading">
      <img src="../assets/loading.gif">
    </div>
    <div id="content" v-else>
      <div class="panel">
        <div class="topic-header">
          <router-link :to="{name:'root'}">
            <a class="homepage" href="javascript:;">主页</a>
          </router-link>
          <span class="divider">/</span>
        </div>
        <div class="inner">
          <div class="person_content">
            <div class="user_big_avatar">
            <img :src="userinfo.avatar_url" :title="userinfo.loginname">
          </div>
          <a class="dark" href="javascript:;">{{userinfo.loginname}}</a>
          </div>
          <ul class="unstyled">
            <span class="big">{{userinfo.score}} 积分</span>
            <li>
              <a class="dark" href="javascript:;">话题收藏</a>
            </li>
            <li>
              <a class="dark" :href="'https://github.com/'+userinfo.githubUsername">
                <img src="../assets/github.png" alt="">
                @{{userinfo.githubUsername}}</a>
            </li>
          </ul>
          <p class="col_fade">注册时间 {{userinfo.create_at|formDate}}</p>
        </div>
      </div>
      <div class="panel">
        <div class="topic-header">
          <span class="col_fade">最近创建的话题</span>
        </div>
        <div>
          <ul class="recent-creat">
            <li v-for="(item,index) in userinfo.recent_topics.slice(0 , 5)" :key="index">
              <img :src="item.author.avatar_url "  :title="item.author.loginname">
              <router-link :to="{name:'article_content',params:{id:item.id,name:item.author.loginname}}">
                <span class="tit"><a href="javascript:;">{{item.title}}</a></span>
              </router-link>
              <span class="time">{{item.last_reply_at|formDate}}</span>
            </li>
            <li v-if="userinfo.recent_replies.length>5"><a class="dark" href="javascript:;">查看更多»</a></li>
          </ul>
        </div>
      </div>
      <div class="panel">
        <div class="topic-header">
          <span class="col_fade">最近参与的话题</span>
        </div>
        <div>
          <ul class="recent-creat">
            <li v-for="(item,index) in userinfo.recent_replies.slice(0 , 5)" :key="index">
              <router-link :to="{name:'userinfo_content',params:{name:item.author.loginname}}">
                <img :src="item.author.avatar_url "  :title="item.author.loginname">
              </router-link>
              <router-link :to="{name:'article_content',params:{id:item.id,name:item.author.loginname}}">
                <span class="tit"><a href="javascript:;">{{item.title}}</a></span>
              </router-link>
              <span class="time">{{item.last_reply_at|formDate}}</span>
            </li>
            <li v-if="userinfo.recent_replies.length>5"><a class="dark" href="javascript:;">查看更多»</a></li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'UserInfo',
  data () {
    return {
      isloading: false,
      userinfo:{}
    }
  },
  methods:{
    getUserInfoData(){
      this.$http.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
      .then(res => {
        this.isloading=false;
        this.userinfo = res.data.data
      })
      .catch(err => {
        console.log(err)
      })
    }
  },
  watch:{
    $route(to,from){
      this.getUserInfoData()
    }
  },
  beforeMount:function(){
    this.isloading=true;
    this.getUserInfoData()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
  @import url('../assets/markdown.css');
  .loading{
    position: fixed;
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%);
  }
  .loading img{
    width: 200px;
  }
  #content {
    padding: 0;
    margin-right: 305px;
    font-size: 14px;
  }
  .panel{
    background-color: #fff;
    margin-bottom: 13px;
    border-radius: 5px 5px 0 0;
    overflow: hidden;
  }
  .panel .first-header{
    padding: 10px;
  }
  #content .topic_full_title {
    font-size: 22px;
    font-weight: 700;
    margin: 8px 0;
    display: inline-block;
    vertical-align: bottom;
    width: 75%;
    line-height: 130%;
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
  #content .changes{
    font-size: 12px;
    color: #838383;
  }
  #content .changes span::before {
    content: "•";
  }
  .panel .inner{
    padding: 10px;
    border-top: 1px solid #e5e5e5;
  }
  .topic_content{
    margin: 0 10px;
  }
  .topic_content p {
    font-size: 15px;
    line-height: 1.7em;
    overflow: auto;
  }
  .topic-header{
    font-size: 14px;
    padding: 10px;
    background-color: #fcfcfc;
  }
  .A-reply{
    padding: 10px;
    border-bottom: 1px #f0f0f0 solid;
  }
  .author_content a>img{
    height: 30px;
  }
  .author_content .user_avatar{
    float: left;
  }
  .user_info{
    margin-left: 10px;
    display: inline-block;
  }
  #content a.dark:hover{
    color: #385f8a;
  }
  #content a.dark{
    color: #666;
    text-decoration: none;
  }
  .reply_author{
    font-size: 12px;
    font-weight: 700;
  }
  .reply_time{
    font-size: 11px;
    margin-left: 5px;
  }
  #content .author_content{
    font-size: 0;
  }
  .user_action {
    float: right;
    margin-left: 20px;
    font-size: 15px;
  }
  .user_action a>img{
    width: 15px;
    height: 15px;
    vertical-align: middle;
  }
  .user_action span{
    color: gray;
    vertical-align: middle;
  }
  .reply_content {
    padding-left: 50px;
    padding-top: 10px;
    color: #333;
  }
  #content .homepage{
    color: #80bd01;
  }
  #content .homepage:hover{
    text-decoration: underline;
  }
  .divider {
    padding: 0 5px;
    color: #ccc;
  }
  .user_big_avatar{
    float: left;
    margin-right: 10px;
  }
  .user_big_avatar img{
    width: 40px;
  }
  #content ul.unstyled {
    margin-left: 0;
    margin-top: 10px;
    padding: 0;
    list-style: none;
  }
  #content ul.unstyled li{
    margin: 10px 0;
  }
  .big {
    font-size: 14px;
  }
  .person_content{
    height: 40px;
  }
  ul.unstyled li img{
    width: 20px;
    height: 20px;
    vertical-align: middle;
  }
  .col_fade {
    color: #ababab;
  }
  .topic-header .col_fade{
    color: #444;
  }
  #content .recent-creat{
    padding: 0;
  }
  #content .recent-creat{
    margin: 0;
  }
  .recent-creat li{
    padding: 10px;
    border: 1px #f0f0f0 solid;
    border-bottom:none;
  }
  .recent-creat li:first-child{
    border-top: 1px #f0f0f0 solid;
  }
  .recent-creat li img{
    height: 30px;
    border-radius:3px ;
    vertical-align: middle;
    display: inline-block;
  }
  .recent-creat li span{
    display: inline-block;
    vertical-align: middle;
  }
  .tit{
    display: inline-block;
    width: 80%;
  }
  .tit a{
    display: inline-block;
    width: 80%;
    font-size: 16px;
    color: #333;
    text-decoration: none;
    line-height: 30px;
    margin-left: 10px;
    text-overflow: ellipsis;
    overflow: hidden;
    white-space:nowrap;
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
</style>
