<template>
  <div id="app">
    <!-- Toggle Button -->
    <div class="toggle-container">
      <label class="toggle-label">Local Save:</label>
      <button
        class="toggle-button"
        :class="{ active: saveEnabled }"
        @click="toggleSave"
      >
        {{ saveEnabled ? 'ON' : 'OFF' }}
      </button>
    </div>

    <!-- Editor -->
    <div
      ref="editor"
      contenteditable="true"
      class="editor"
      :data-placeholder="placeholderText"
      @input="handleInput"
      @keydown="handleKeydown"
    ></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      noteContent: localStorage.getItem('noteContent') || '',
      placeholderText: 'Start typing your notes here...',
      saveEnabled: true // Default to saving enabled
    };
  },
  mounted() {
    this.$refs.editor.innerHTML = this.noteContent;
    this.updatePlaceholder();
  },
  methods: {
    handleInput() {
      this.noteContent = this.$refs.editor.innerHTML;
      if (this.saveEnabled) {
        localStorage.setItem('noteContent', this.noteContent);
      }
      this.updatePlaceholder();
    },
    updatePlaceholder() {
      const editor = this.$refs.editor;
      if (editor.innerHTML === '' || editor.innerHTML === '<br>') {
        editor.setAttribute('data-placeholder', this.placeholderText);
      } else {
        editor.removeAttribute('data-placeholder');
      }
    },
    handleKeydown(event) {
      // Ctrl + B for Bold
      if (event.ctrlKey && event.key === 'b') {
        event.preventDefault();
        document.execCommand('bold', false, null);
      }
      // Ctrl + I for Italic
      else if (event.ctrlKey && event.key === 'i') {
        event.preventDefault();
        document.execCommand('italic', false, null);
      }
    },
    toggleSave() {
      this.saveEnabled = !this.saveEnabled;
      if (!this.saveEnabled) {
        // Clear localStorage when saving is disabled
        localStorage.removeItem('noteContent');
      } else {
        // Save current content to localStorage when saving is enabled
        localStorage.setItem('noteContent', this.noteContent);
      }
    }
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100..900;1,100..900&display=swap');

body, html {
  margin: 0;
  padding: 0;
  height: 100%;
  overflow: hidden; /* Eliminate the browser scroll bar */
  background-color: #f4f2ee;
}

#app {
  height: 100%;
  display: flex;
  flex-direction: column;
}

.toggle-container {
  padding: 10px 20px;
  background-color: #fff;
  border-bottom: 1px solid #ddd;
  display: flex;
  align-items: center;
  gap: 10px;
}

.toggle-label {
  font-family: "Montserrat", serif;
  font-size: 16px;
  color: #333;
}

.toggle-button {
  padding: 5px 15px;
  font-family: "Montserrat", serif;
  font-size: 14px;
  color: #333;
  background-color: #f4f2ee;
  border: 1px solid #ccc;
  border-radius: 20px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.toggle-button.active {
  background-color: #4caf50; /* Green when ON */
  color: #fff;
}

.editor {
  width: 100%;
  flex-grow: 1;
  border: none;
  outline: none;
  padding: 20px;
  font-size: 20px;
  font-family: "Montserrat", serif;
  background-color: #f4f2ee;
  color: #333;
  box-sizing: border-box;
  white-space: pre-wrap; /* Preserve formatting like line breaks */
  line-height: 1.5; /* Consistent line height */
}

.editor[data-placeholder]:empty::before {
  content: attr(data-placeholder);
  color: #999;
  font-style: italic;
}

/* Custom scrollbar */
.editor::-webkit-scrollbar {
  width: 15px;
}
.editor::-webkit-scrollbar-thumb {
  background-color: #ccc;
  border-radius: 20px;
}
.editor::-webkit-scrollbar-track {
  background-color: #f4f2ee;
}
</style>