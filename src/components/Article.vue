<template>
  <div>
    <div class="loading" v-if="isloading">
      <img src="../assets/loading.gif">
    </div>
    <div id="content" v-else>
      <div class="panel">
        <div class="first-header">
          <span class="topic_full_title">
            <span :class="[{put_good:(posts.good==true),put_top:(posts.top==true),put_pic:(posts.good!=true&&posts.top!=true)}]">
              <span>{{posts|tabFormation}}</span>
            </span>
            {{posts.title}}</span>
          <div class="changes">
            <span> 发布于 {{posts.create_at|formDate}}</span>
            <span> 作者 {{posts.author.loginname}}</span>
            <span>{{posts.visit_count}} 次浏览</span>
            <span> 最后一次编辑是 {{posts.last_reply_at|formDate}}</span>
            <span> 来自 {{posts|tabFormation}}</span>
          </div>
        </div>
        <div class="inner">
          <div v-html="posts.content" class="topic_content"></div>
        </div>
      </div>
      <div class="panel">
        <div class="topic-header">{{posts.reply_count}} 回复</div>
        <div class="A-reply" v-for="(list,index) in posts.replies" :key="index">
          <div class="author_content">
            <router-link :to="{name:'userinfo_content',params:{name:list.author.loginname}}">
              <a class="user_avatar" href="javascript:;">
                <img :src="list.author.avatar_url" :title="list.author.loginname">
              </a>
            </router-link>
            <div class="user_info">
              <router-link :to="{name:'userinfo_content',params:{name:list.author.loginname}}">
                <a class="dark reply_author" href="javascript:;">{{list.author.loginname}}</a>
              </router-link>
              <a class="reply_time" href="javascript:;"> {{index+1}}楼•{{list.create_at|formDate}}</a>
            </div>
            <div class="user_action" v-if="list.ups.length>0">
              <a href="javascript:;"><img src="../assets/zan.png" title="喜欢"></a>
              <span>{{list.ups.length}}</span>
            </div>
          </div>
          <div v-html="list.content" class="reply_content"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Article',
  data () {
    return {
      isloading: false,
      posts:{}
    }
  },
  methods:{
    getArticleData(){
      this.$http.get(`https://cnodejs.org/api/v1/topic/${this.$route.params.id}`)
      .then(res => {
        this.isloading=false;
        this.posts = res.data.data
      })
      .catch(err => {
        console.log(err)
      })
    }
  },
  watch:{
    $route(to,from){
      this.getArticleData()
    }
  },
  beforeMount:function(){
    this.isloading=true;
    this.getArticleData()
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
    width: 100%;
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
</style>
