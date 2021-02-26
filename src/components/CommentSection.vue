<template>
  <div class="comment-section">
    <CommentForm @commentSent="getComments"></CommentForm>
    <div class="comments">
      <Comment
        :key="comment.id"
        v-for="comment in comments"
        v-bind:comment="comment"
      ></Comment>
    </div>
  </div>
</template>
<script>
import CommentForm from "./CommentForm";
import Comment from "./Comment";

export default {
  name: "CommentSection",
  components: {
    CommentForm,
    Comment,
  },
  data() {
    return {
      comments: [],
      error: "",
    };
  },
  created() {
    this.getComments();
  },
  methods: {
    async getComments() {
      try {
        const response = await fetch("/api/v1/comments");
        const jsonData = await response.json();
        this.comments = jsonData.data;
      } catch (err) {
        this.error = err.message;
      }
    },
  },
};
</script>
<style>
.comments {
  display: flex;
  flex-direction: column;
  overflow: auto;
  align-items: flex-start;
  justify-content: flex-start;
  width: 100%;
  height: 40%;
}

.comment-section {
  height: 100vh;
}
</style>