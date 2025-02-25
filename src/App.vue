<template>
  <div id="app">
    <textarea
      v-model="noteContent"
      placeholder="Start typing your notes here..."
      @input="saveNote"
    ></textarea>
  </div>
</template>

<script>
export default {
  data() {
    return {
      noteContent: localStorage.getItem('noteContent') || ''
    };
  },
  methods: {
    saveNote() {
      localStorage.setItem('noteContent', this.noteContent);
    }
  }
};
</script>

<style>
body, html {
  margin: 0;
  padding: 0;
  height: 100%;
  background-color: #f4f2ee;
}

#app {
  height: 100%;
}

textarea {
  width: 100%;
  height: 100%;
  border: none;
  outline: none;
  resize: none;
  padding: 20px;
  font-size: 20px;
  font-family: "Montserrat", serif;
  background-color: #f4f2ee;
  color: #333;
  box-sizing: border-box;
}
</style>