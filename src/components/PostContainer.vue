<template>
  <div class="post-container">
    <Postcard/>
    <Postcard/>
    <Postcard/>
    <Postcard/>
  </div>
</template>

<script setup>
import Postcard from './PostCard.vue'
</script>

<style>
.post-container{
  width: 100%;
  display: flex;
  flex-wrap: wrap;
}

</style>
