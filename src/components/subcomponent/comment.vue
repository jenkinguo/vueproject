<template>
   <div class="cmt-container">
      <h3>发表评论</h3>
      <hr>
      <textarea placeholder="请输入内容" maxlength="120" v-model="msg"></textarea>

      <mt-button size="large" type="primary" @click="postComment">发表评论</mt-button>
      <!-- 评论内容 -->
      <div class="cmt-list">
         <div class="cmt-item" v-for="(item,i) in comments" :key="item.add_time">
            <div class="cmt-title">
               第{{ i+1 }}楼&nbsp;&nbsp;用户: {{ item.user_name }}&nbsp;&nbsp;发表时间: {{ item.add_time | dateFormat }}
            </div>
            <div class="cmt-body">
               {{ item.content === "undefined" ? '此用户没有评论' : item.content}}
            </div>
         </div>
      </div>

      <mt-button size="large" type="danger" plain @click="getMore">加载更多</mt-button>
   </div>
</template>

<script>
   import { Toast } from 'mint-ui';

   export default {
      data() {
         return {
            pageindex: 1,
            comments: [],
            msg: ""
         }
      },
      created() {
         this.getcomments();
      },
      methods: {
         getComments() {
            this.$http.get("api/getcomments/"+ this.id +"?pageindex=" + this.pageindex).then(result => {
               if (result.body.status === 0) {
                  // this.comments = result.body.message;
                  // 应使用数组拼接
                  this.comments = this.comments.concat(result.body.message);
               } else {
                  Toast('获取失败');
               }
            })
         },
         getMore() { //更多评论
            this.pageindex++;
            this.getcomments();
         },
         postComment() {
            //校验评论不能为空
            if (this.msg.trim().length === 0) {
               return Toast('评论不能为空');
            }
            //发表评论
            this.$http.post('api/postcomment/' + this.$router.params.id, {content: this.msg.trim()}).then(result => {
               if (result.body.status === 0) {
                  //1.拼接处一个对象
                  var cmt = {
                     user_name: "匿名用户",
                     add_time: Date.now(),
                     content: this.msg.trim()
                  };
                  this.comments.unshift(cmt);
                  this.msg = "";
               }
            });
         }
      },
      props: ["id"]
   }
</script>

<style lang="scss" scoped>
   .cmt-container {
      h3 {
         font-size: 18px;
      }
      textarea {
         font-size: 14px;
         height: 85px;
         margin: 0;
      }

      .cmt-list {
         margin: 5px 0;
         .cmt-item {
            font-size: 13px;
            .cmt-title {
               line-height: 30px;
               background-color: #ccc;
            }
            .cmt-body {
               line-height: 35px;
               text-indent: 2em;
            }
         }
      }
   }
</style>