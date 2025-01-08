<template>
  <div>
    <div v-if="result">
      <div v-for="image in filteredImages" :key="image.id">
        <img
          :src="image.url"
          :alt="image.name"
          style="width: 100px; height: auto"
        />
        <p>{{ image.name }}</p>
      </div>
    </div>
    <div v-else-if="searchTerm">
      <p>沒有找到相關圖片。</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    filteredImages: Array,
    hasResults: Boolean,
    result: String,
  },
};
</script>
