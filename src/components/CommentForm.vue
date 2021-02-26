<template>
  <div class="send-comment">
    <form @submit.prevent="onSubmit" class="comment-form">
      <input placeholder="Name" v-model="comment.name" type="text" />
      <textarea
        placeholder="Message"
        v-model="comment.text"
        name="text"
        id="text"
        rows="8 "
      ></textarea>
      <input type="submit" value="Send" />
    </form>
    <p class="error">{{ error }}</p>
  </div>
</template>
<script>
export default {
  name: "CommentForm",
  data() {
    return {
      comment: {
        name: "",
        text: "",
      },
      error: "",
    };
  },
  methods: {
    onSubmit() {
      if (!this.comment.name || !this.comment.name.trim()) {
        this.error = "Write your name please.";
        return;
      }
      if (!this.comment.text || !this.comment.text.trim()) {
        this.error = "Write your comment please.";
        return;
      }
      this.sendComment();
      this.clearInputs();
    },
    clearInputs() {
      this.comment.name = "";
      this.comment.text = "";
    },
    async sendComment() {
      const body = JSON.stringify(this.comment);
      try {
        await fetch("/api/v1/comments", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body,
        });
        this.$emit("commentSent");
      } catch (err) {
        this.error = err.message;
      }
    },
  },
};
</script>
<style>
.send-comment {
  box-sizing: border-box;
}

.comment-form {
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  width: 100%;
  padding: 1rem;
}

.comment-form input {
  background: rgb(32, 30, 30);
  color: #fff;
  width: 100%;
  margin: 1rem 0;
  border: #f4f4f4 1px solid;
  padding: 0.5rem 1rem;
  box-sizing: border-box;
}

#text {
  resize: none;
  width: 100%;
  padding: 1rem;
  background: rgb(32, 30, 30);
  color: #fff;
  border: #f4f4f4 1px solid;
  box-sizing: border-box;
}
</style>