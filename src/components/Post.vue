<template>
  <div class="post">
    <ImageSection></ImageSection>
    <CommentSection></CommentSection>
  </div>
</template>

<script>
import ImageSection from './ImageSection.vue'
import CommentSection from './CommentSection.vue'

export default {
  name: 'post',
  components: {
    ImageSection,
    CommentSection
  }
}
</script>

<style scoped>
.post {
  text-align: center;
}
</style>
