<template>
  <div class="file-block">
    <transition name="shiftx">
      <form v-show="formShow" @submit.prevent="uploadFile" class="main-form">
        <label class="button" for="audio-file">
          <input
            @change="onFileChange"
            type="file"
            name="audio-file"
            id="audio-file"
          />
          Upload File
        </label>
        <select
          v-model="fileType"
          class="ext-select"
          name="file-ext"
          id="file-ext"
        >
          <option selected value="flac">flac</option>
          <option value="wav">wav</option>
          <option value="vox">vox</option>
        </select>
        <input
          class="button submit-button"
          v-if="!loading"
          type="submit"
          value="Convert File"
        />
        <p class="error">{{ error }}</p>
      </form>
    </transition>

    <transition name="shiftrevX">
      <img
        v-show="loading"
        src="@/assets/92.gif"
        alt="Loading..."
        class="loading"
      />
    </transition>

    <transition name="shiftrevX">
      <div class="download-block" v-show="fileLink">
        <a class="button download" :href="fileLink"> Download File </a>
        <button class="button" @click="showForm">Convert more</button>
      </div>
    </transition>
  </div>
</template>
<script>
export default {
  name: "FileInputForm",
  data() {
    return {
      fileType: "flac",
      audioFile: null,
      fileLink: null,
      loading: false,
      formShow: true,
      error: "",
    };
  },
  methods: {
    uploadFile() {
      this.error = "";
      if (!this.audioFile) {
        this.error = "Upload a file please";
        return;
      }
      this.formShow = false;
      this.loading = true;
      this.sendForm(this.formData());
    },
    onFileChange(e) {
      const files = e.target.files || e.dataTransfer.files;
      if (!files.length) return;
      this.audioFile = files[0];
    },
    async sendForm(data) {
      try {
        const response = await fetch("/api/v1/audio/process", {
          method: "POST",
          body: data,
        });
        if (response.status != 200) {
          const jsonResponse = await response.json();
          this.error = jsonResponse.error;
          this.formShow = true;
        } else {
          const jsonResponse = await response.json();
          this.fileLink = jsonResponse.data;
        }
      } catch (err) {
        this.error = err.message;
        this.formShow = true;
      }
      this.loading = false;
    },
    formData() {
      const formData = new FormData();
      formData.append("type", this.fileType);
      formData.append("audioFile", this.audioFile);
      return formData;
    },
    showForm() {
      this.fileLink = null;
      this.formShow = true;
    },
  },
};
</script>
<style scoped>
.shiftx-leave-active {
  animation: shift-right 1.5s ease-in-out;
}

.shiftx-enter-active {
  animation: shift-right 1.5s ease-in-out reverse;
}

@keyframes shift-right {
  0% {
    transform: translateX(0px);
  }
  100% {
    transform: translateX(1000px);
  }
}

.shiftrevX-leave-active {
  animation: shift-left 1.5s ease-in-out;
}

.shiftrevX-enter-active {
  animation: shift-left 1.5s ease-in-out reverse;
}

@keyframes shift-left {
  0% {
    transform: translateX(0px);
  }
  100% {
    transform: translateX(-1000px);
  }
}

.file-block {
  overflow: hidden;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.main-form {
  padding: 0 3rem;
  display: flex;
  flex-direction: column;
  text-align: center;
}

.file-upload:hover {
  cursor: pointer;
}

.ext-select {
  margin: 1rem 2rem;
  padding: 1rem 3rem;
  border: none;
  outline: none;
  text-align: center;
  text-align-last: center;
  font-size: 1rem;
}

.ext-select option {
  text-align: center;
}

.loading {
  width: 3rem;
  height: 3rem;
  position: absolute;
  align-self: center;
}

.download {
  margin: auto 3rem;
  padding: 1rem 3rem;
  text-decoration: none;
}

.download-block {
  position: absolute;
}

#audio-file {
  display: none;
}
</style>