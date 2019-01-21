<template>
  <div class="comment">
    <!-- Edit Mode -->
    <CommentEdit v-if="edit" @updateComment="updateComment" @cancelEdit="cancelEdit" :username="comment.username" :text="comment.text" ></CommentEdit>
    <!-- Default Display -->
    <div v-else>
      <h4 class="commenter">Commenter: {{ comment.username }}</h4>
      <h4 class="comment-header">Comment:</h4>
      <div>{{ comment.text }}</div>
    </div>
    <!-- Navigation -->
    <div class="navigation">
      <div @click="replyToComment" class="nav-option"><font-awesome-icon icon="reply" /><div class="nav-name">Reply</div></div>
      <div @click="editComment" class="nav-option"><font-awesome-icon icon="edit" /><div class="nav-name">Edit</div></div>
      <div @click="deleteComment" class="nav-option"><font-awesome-icon icon="trash" /><div class="nav-name">Trash</div></div>
    </div>
    <!-- Reply Mode -->
    <CommentReply v-if="reply" @addReply="addReply" @cancelReply="cancelReply"></CommentReply>
    <!-- Replies -->
    <div class="replies">
      <Comment v-for="(comment, index) in comments" :key="index" @updateComment="updateReply" @deleteComment="deleteReply" :comment="comment"></Comment>
    </div>
  </div>
</template>

<script>
import CommentEdit from './CommentEdit.vue'
import CommentReply from './CommentReply.vue'
import Comment from './Comment.vue'

export default {
  name: 'comment',
  components: {
    CommentEdit,
    CommentReply,
    Comment: () => import('./Comment.vue')
  },
  props: {
    comment: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      edit: false,
      reply: false,
      comments: []
    };
  },
  methods: {
    // Edit Work Flow:
    editComment() {
      this.reply = false;
      this.edit = true;
    },
    updateComment(commentData) {
      commentData.id = this.comment.id;
      this.edit = false;

      const index = this.comments.findIndex(current => {
        return current.id === commentData.id;
      });

      this.$emit('updateComment', commentData);
    },
    cancelEdit() {
      this.edit = false;
    },

    // Delete Work Flow:
    deleteComment(id) {
      const index = this.comments.findIndex(current => {
        return current.id === id;
      });

      this.$emit('deleteComment', this.comment.id);
    },

    // Reply Work Flow:
    replyToComment() {
      this.edit = false;
      this.reply = true;
    },
    addReply(commentData) {
      commentData.id = Math.random();
      this.comments.unshift(commentData);
      this.reply = false;
    },
    cancelReply() {
      this.reply = false;
    },

    // Update Reply Work Flow:
    updateReply(commentData) {
      const index = this.comments.findIndex(current => {
        return current.id === commentData.id;
      });

      this.comments[index].username = commentData.username;
      this.comments[index].text = commentData.text;
    },

    // Delete Reply Work Flow:
    deleteReply(id) {
      const index = this.comments.findIndex(current => {
        return current.id === id;
      });

      this.comments.splice(index, 1);
    }
  }
}
</script>

<style scoped>
.comment {
  margin-bottom: 30px;
}

.commenter {
  color: dimgray;
  margin: 0px 0px 10px 0px;
}

.comment-header {
  margin: 0px 0px 10px 0px;
}

.navigation {
  border: 1px solid dimgray;
  border-radius: 5px;
  color: dimgray;
  margin-top: 10px;
  padding: 5px 10px;
  cursor: pointer;
  width: fit-content;
}

.nav-option {
  display: inline-block;
  margin-right: 15px;
}

.nav-name {
  margin-left: 15px;
  display: inline-block;
}

.replies {
  border-left: 1px solid dimgrey;
  padding-left: 15px;
  margin-left: 15px;
  margin-top: 30px;
}
</style>
