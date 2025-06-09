<template>
<div class = "post-card">
  <h3>Titulo</h3>
  <p> Lorem ipsum dolor sit amet, consectetur adipisicing elit. Laborum animi error impedit velit consequuntur cumque temporibus recusandae eius, et nam in explicabo fugit quisquam accusantium nulla ut quaerat reprehenderit voluptatem.</p>
  <PostButton />
</div>
</template>

<script setup>
import PostButton from './PostButton.vue'
</script>

<style  scoped>
.post-card {
  border-radius: 15px;
  margin: 5px;
  border: 1px solid #ccc;
}
</style>
