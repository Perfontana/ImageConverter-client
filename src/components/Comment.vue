<template>
  <article class="comment">
    <span class="comment-head">
      <h1 class="name">{{ comment.name }}</h1>
      <span class="time">{{ new Date(comment.date).toString() }}</span>
    </span>
    <p v-html="comment.text"></p>
  </article>
</template>
<script>
export default {
  name: "Comment",
  props: ["comment"],
};
</script>
<style>
.comment {
  color: #fff;
}

.comment-head h1 {
  display: inline-block;
  margin-right: 2rem;
}
</style>