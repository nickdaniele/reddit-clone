<template>
  <div class="comment-submit">
    <!-- Username Input -->
    <label for="username">User Name:</label>
    <input v-model.trim="username" type="text" class="username" name="username" placeholder="Enter your user name here...">
    <div v-if="username_error" class="error">Please enter your user name.</div>
    <!-- Comment Textarea -->
    <label for="comment-text">Comment:</label>
    <textarea v-model.trim="comment_text" class="comment-text" name="comment-text" placeholder="Enter your comment here..."></textarea>
    <div v-if="comment_text_error" class="error">Please enter your comment.</div>
    <!-- Submit -->
    <button @click="validate" class="submit" type="button">Post</button>
  </div>
</template>

<script>
export default {
  name: 'comment-submit',
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
    validate() {
      const username = this.username;
      const text = this.comment_text;

      if (username === null) this.username_error = true;
      if (text === null) this.comment_text_error = true;

      this.addComment();
    },
    addComment() {
      if (!this.username_error && !this.comment_text_error) {
        const username = this.username;
        const text = this.comment_text;

        this.username = null;
        this.comment_text = null;

        this.$emit('addComment', { username, text });
      }
    }
  }
}
</script>

<style scoped>
.comment-submit {
  margin-bottom: 25px;
}

label, .submit {
  display: block;
}

label {
  margin-bottom: 8px;
  margin-top: 15px;
}

.username {
  width: 200px;
  margin-bottom: 15px;
}

.comment-text {
  width: 300px;
  margin-bottom: 15px;
}

.submit {
  padding: 5px 9px;
  border-radius: 5px;
  background-color: lightskyblue;
  color: white;
  border: 1px solid blueviolet;
  cursor: pointer;
  margin-top: 10px;
}

.error {
  color: red;
  font-size: 14px;
}
</style>
