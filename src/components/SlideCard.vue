<template>
    <div class="card">
      <div v-for="(option, index) in options" :key="index" :class="optionClass(index)" class="option-item">
        {{ option }}
      </div>
      <div class="selected-option" v-if="options.length > 0">
        Selected: {{ options[activeOptionIndex] }}
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      options: Array,
      isActive: Boolean,
      activeOptionIndex: Number,
    },
    data() {
      return {
        currentActiveIndex: this.activeOptionIndex,
      };
    },
    watch: {
      isActive(newVal) {
        if (newVal) {
          window.addEventListener('keydown', this.handleKeyDown);
          this.currentActiveIndex = this.activeOptionIndex;
        } else {
          window.removeEventListener('keydown', this.handleKeyDown);
        }
      },
      activeOptionIndex(newVal) {
        this.currentActiveIndex = newVal;
      },
    },
    methods: {
      handleKeyDown(event) {
        if (event.key === 'ArrowUp') {
          this.currentActiveIndex = (this.currentActiveIndex - 1 + this.options.length) % this.options.length;
          this.$emit('updateActiveOptionIndex', this.currentActiveIndex);
        } else if (event.key === 'ArrowDown') {
          this.currentActiveIndex = (this.currentActiveIndex + 1) % this.options.length;
          this.$emit('updateActiveOptionIndex', this.currentActiveIndex);
        }
      },
      optionClass(index) {
        if (index === this.currentActiveIndex && this.isActive) {
          return 'highlighted-option';
        }
        if (index === this.activeOptionIndex) {
          return 'selected-option-item';
        }
        return '';
      }
    },
    mounted() {
      if (this.isActive) {
        window.addEventListener('keydown', this.handleKeyDown);
      }
    },
    beforeDestroy() {
      window.removeEventListener('keydown', this.handleKeyDown);
    },
  };
  </script>
  
  <style>
  .card {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  
  .option-item {
    padding: 10px;
    margin: 5px;
    border: 1px solid #ccc;
    border-radius: 4px;
    cursor: pointer;
    width: 100%;
    text-align: center;
  }
  
  .option-item.highlighted-option {
    background-color: #ffeb3b; /* Dark yellow */
  }
  
  .option-item.selected-option-item {
    background-color: #fff9c4; /* Light yellow */
  }
  
  .selected-option {
    margin-top: 10px;
    font-weight: bold;
  }
  </style>
  