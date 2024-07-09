<template>
  <div class="container">
    <div v-for="(slide, index) in slides" :key="index" :class="{ active: activeSlide === index }" class="slide">
      <SlideCard
        :options="slide.options"
        :isActive="activeSlide === index"
        :activeOptionIndex="slide.activeOptionIndex"
        @updateActiveOptionIndex="updateActiveOptionIndex"
      />
    </div>
  </div>
</template>

<script>
import SlideCard from './components/SlideCard.vue';

export default {
  components: {
    SlideCard,
  },
  data() {
    return {
      slides: [
        { options: ['Option 1.1', 'Option 1.2', 'Option 1.3'], activeOptionIndex: 0 },
        { options: ['Option 2.1', 'Option 2.2', 'Option 2.3'], activeOptionIndex: 0 },
        { options: ['Option 3.1', 'Option 3.2', 'Option 3.3'], activeOptionIndex: 0 },
        { options: ['Option 4.1', 'Option 4.2', 'Option 4.3'], activeOptionIndex: 0 },
      ],
      activeSlide: 0,
    };
  },
  methods: {
    handleKeyDown(event) {
      if (event.key === 'ArrowRight') {
        this.activeSlide = (this.activeSlide + 1) % this.slides.length;
      } else if (event.key === 'ArrowLeft') {
        this.activeSlide = (this.activeSlide - 1 + this.slides.length) % this.slides.length;
      }
    },
    updateActiveOptionIndex(activeOptionIndex) {
      this.$set(this.slides[this.activeSlide], 'activeOptionIndex', activeOptionIndex);
    }
  },
  mounted() {
    window.addEventListener('keydown', this.handleKeyDown);
  },
  beforeDestroy() {
    window.removeEventListener('keydown', this.handleKeyDown);
  },
};
</script>

<style>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  overflow: hidden;
  background-color: #f8f9fa;
}

.slide {
  width: 200px;
  padding: 20px;
  margin: 10px;
  border: 1px solid #ccc;
  border-radius: 8px;
  background-color: white;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  align-items: center;
}

.slide.active {
  border: 2px solid #007bff;
}
</style>
