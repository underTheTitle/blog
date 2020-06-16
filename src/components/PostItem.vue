<template>
  <div class="post-item">
    <span class="date">{{date}}</span>
    <g-link :to="post.path" class="read">{{post.title}}</g-link>
  </div>
</template>

<script>
export default {
  props: ["post"],
  computed: {
    date() {
      return this.post.date.substring(0, this.post.date.length - 5);
    }
  }
};
</script>

<style>
.date {
  margin-right:10px;
  min-width: 60px;
  display:inline-block;
}
</style>
