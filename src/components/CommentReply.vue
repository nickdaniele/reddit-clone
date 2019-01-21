<template>
  <div class="comment-submit">
    <!-- Username Input -->
    <label for="username">Commenter:</label>
    <input v-model.trim="username" type="text" class="username" name="username" placeholder="Enter your user name here...">
    <div v-if="username_error" class="error">Please enter your user name.</div>
    <!-- Comment Textarea -->
    <label for="comment-text" class="comment-label">Comment:</label>
    <textarea v-model.trim="comment_text" class="comment-text" name="comment-text" placeholder="Enter your comment here..."></textarea>
    <div v-if="comment_text_error" class="error">Please enter your comment.</div>
    <!-- Submit -->
    <div class="edit-nav">
      <button @click="cancel"  class="cancel" type="button">Cancel</button>
      <button @click="validate"  class="submit" type="button">Save</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'comment-reply',
  data() {
    return {
      username: null,
      username_error: null,
      comment_text: null,
      comment_text_error: null
    }
  },
  watch: {
    username: function(name) {
      let error = false;
      if (name !== null && name.length === 0) error = true;
      this.username_error = error;
    },
    comment_text: function(text) {
      let error = false;
      if (text !== null && text.length === 0) error = true;
      this.comment_text_error = error;
    }
  },
  methods: {
    cancel() {
      this.username = null;
      this.comment_text = null;
      this.$emit('cancelReply');
    },
    validate() {
      const username = this.username;
      const text = this.comment_text;

      if (username === null) this.username_error = true;
      if (text === null) this.comment_text_error = true;

      this.addReply();
    },
    addReply() {
      if (!this.username_error && !this.comment_text_error) {
        const username = this.username;
        const text = this.comment_text;

        this.username = null;
        this.comment_text = null;

        this.$emit('addReply', { username, text });
      }
    }
  }
}
</script>

<style scoped>
.label {
  display: inline-block;
  margin-right: 10px;
  margin-bottom: 10px;
}

.comment-submit {
  margin-top: 30px;
}

.username {
  width: 150px;
}

.comment-next, .edit-nav {
  display: block; 
}

.comment-label {
  display: block;
}

.comment-text {
  margin-top: 10px;
  margin-bottom: 10px;
  width: 235px;
}

.cancel, .submit {
  display: inline-block;
}

.username {
  margin-bottom: 15px;
  margin-left: 10px;
}

.error {
  color: red;
}
</style>
