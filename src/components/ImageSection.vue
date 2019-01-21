<template>
    <div class="image-content">
      <h1 class="title">{{ title }}</h1>
      <h4 class="username">Posted By: {{ username }}</h4>
      <img class="image" :src="image_path">
    </div>
</template>

<script>
export default {
  name: 'image-section',
  data() {
    return {
      title: 'Laser Cats',
      username: 'PapayaWhip',
      image: 'laser_cats.jpg'
    }
  },
  computed: {
    image_path() {
      return require(`../assets/${this.image}`);
    }
  }
}
</script>

<style scoped>
.image-content {
  display: inline-block;
  text-align: left;
  margin-right: 30px;
}

.username {
  margin-top: 0px;
  color: dimgray;
}

.image {
  width: 400px;
}
</style>
