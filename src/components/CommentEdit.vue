<template>
  <div class="comment-submit">
    <!-- Username Input -->
    <label for="username">Commenter:</label>
    <input v-model.trim="data_username" type="text" class="username" name="username" placeholder="Enter your user name here...">
    <div v-if="username_error" class="error">Please enter your user name.</div>
    <!-- Comment Textarea -->
    <label for="comment-text" class="comment-label">Comment:</label>
    <textarea v-model.trim="data_comment_text" class="comment-text" name="comment-text" placeholder="Enter your comment here..."></textarea>
    <div v-if="comment_text_error" class="error">Please enter your comment.</div>
    <!-- Submit -->
    <div class="edit-nav">
      <button @click="cancel" class="cancel" type="button">Cancel</button>
      <button @click="validate" class="submit" type="button">Save</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'comment-edit',
  props: {
    username: {
      required: true,
      type:String
    },
    text: {
      required: true,
      type: String
    }
  },
  data() {
    return {
      data_username: this.username,
      username_error: null,
      data_comment_text: this.text,
      comment_text_error: null
    }
  },
  watch: {
    data_username: function(name) {
      let error = false;
      if (name !== null && name.length === 0) error = true;
      this.username_error = error;
    },
    data_comment_text: function(text) {
      let error = false;
      if (text !== null && text.length === 0) error = true;
      this.comment_text_error = error;
    }
  },
  methods: {
    cancel() {
      this.data_username = this.username;
      this.data_comment_text = this.text;
      this.$emit('cancelEdit');
    },
    validate() {
      const username = this.data_username;
      const text = this.data_comment_text;

      if (username === null) this.username_error = true;
      if (text === null) this.comment_text_error = true;

      this.updateComment();
    },
    updateComment() {
      if (!this.username_error && !this.comment_text_error) {
        const username = this.data_username;
        const text = this.data_comment_text;

        this.data_username = null;
        this.data_comment_text = null;

        this.$emit('updateComment', { username, text });
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
