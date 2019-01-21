<template>
  <div class="comment-section">
    <CommentSubmit @addComment="addComment"></CommentSubmit>
    <Comment v-for="(comment, index) in comments" :key="index" @updateComment="updateComment" @deleteComment="deleteComment" :comment=comment></Comment>
  </div>
</template>

<script>
import CommentSubmit from './CommentSubmit.vue'
import Comment from './Comment.vue'

export default {
  name: 'comment-section',
  components: {
    Comment,
    CommentSubmit
  },
  data() {
      return {
        comments: []
      }
  },
  methods: {
    updateComment(commentData) {
      const index = this.comments.findIndex(current => {
        return current.id === commentData.id;
      });

      this.comments[index].username = commentData.username;
      this.comments[index].text = commentData.text;
    },
    addComment(commentData) {
      commentData.id = Math.random();
      
      this.comments.unshift(commentData);
    },
    deleteComment(id) {
      const index = this.comments.findIndex(current => {
        return current.id === id;
      });

      this.comments.splice(index, 1);
    }
  }
}
</script>

<style scoped>
.comment-section {
  display: inline-block;
  vertical-align: top;
  width: calc(100% - 524px);
  text-align: left;
}
</style>