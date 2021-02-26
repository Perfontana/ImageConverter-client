<template>
  <div class="main-screen">
    <FileInput></FileInput>
    <CommentSection></CommentSection>
  </div>
</template>

<script>
import FileInput from "./components/FileInputForm";
import CommentSection from "./components/CommentSection";

export default {
  name: "App",
  components: {
    FileInput,
    CommentSection,
  },
};
</script>

<style>
body {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  overflow: hidden;

  font-family: Arial, Helvetica, sans-serif;
}

.main-screen {
  display: grid;
  grid-template-columns: 1fr 1fr;
  align-items: center;
  justify-content: center;
  width: 100vw;
  height: 100vh;
  background: rgb(32, 30, 30);
}

.button {
  margin: 1rem 2rem;
  padding: 1rem 3rem;
  display: inline-block;
  background: #ac5d04;
  color: #000000;
  outline: none;
  border: none;
  font-size: 1rem;
  text-align: center;
}

.button:hover {
  cursor: pointer;
}

.error {
  color: #ff7b7b;
}
</style>
