<template>
  <div class="image-content">
    <div class="table-header">
      <div class="header-item">編號</div>
      <div class="header-item">圖片</div>
      <div class="header-item">說明</div>
      <div class="header-item">建立時間</div>
      <div class="header-item">圖片來源</div>
      <div class="header-item">功能</div>
    </div>
    <div v-if="result">
      <div class="image-main" v-for="image in filteredImages" :key="image.id">
        <div class="item">{{ image.id }}</div>
        <div class="item"><img :src="image.url" :alt="image.name" /></div>
        <div class="item">{{ image.content }}</div>
        <div class="item">{{ image.date }}</div>
        <div class="item">{{ image.from }}</div>
        <div class="item">
          <button class="item-btn" @click="previewImage(image.url)">
            預覽
          </button>
          <button class="item-btn" @click="downloadImage(image.url)">
            下載
          </button>
          <button class="item-btn" @click="someFunction(image.id)">複製</button>
        </div>
      </div>
    </div>
    <div v-else>
      <p>沒有找到相關圖片。</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    images: {
      type: Array,
      required: true,
    },
    searchTerm: {
      type: String,
      default: "",
    },
    selectedSources: {
      type: Object,
      required: true,
    },
    itemsPerPage: {
      type: Number,
      default: 1,
    },
    currentPage: {
      type: Number,
      required: true,
    },
  },

  computed: {
    filteredImages() {
      return this.images.filter((image) => {
        const matchesSearchTerm = image.name
          .toLowerCase()
          .includes(this.searchTerm.toLowerCase());
        const matchesSource =
          this.selectedSources.all ||
          (this.selectedSources.rti && image.from === "RTI") ||
          (this.selectedSources.cna && image.from === "CNA") ||
          (this.selectedSources.ap && image.from === "AP") ||
          (this.selectedSources.afp && image.from === "AFP") ||
          (this.selectedSources.program && image.from === "PROGRAM") ||
          (this.selectedSources.company && image.from === "COMPANY") ||
          (this.selectedSources.reuters && image.from === "REUTERS") ||
          (this.selectedSources.others && image.from === "OTHERS");
        return matchesSearchTerm && matchesSource;
      });
    },
    paginatedImages() {
      const start = (this.currentPage - 1) * this.itemsPerPage;
      return this.filteredImages.slice(start, start + this.itemsPerPage);
    },
    result() {
      return this.filteredImages.length > 0;
    },
  },

  methods: {
    previewImage(url) {
      this.$emit("preview", url);
    },
    downloadImage(url) {
      const link = document.createElement("a");
      link.href = url;
      link.download = url.split("/").pop();
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
    },
    copyToClipboard(url) {
      navigator.clipboard.writeText(url).then(() => {});
    },
  },
};
</script>

<style lang="scss">
.image-content {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  .image-main {
    display: flex;
    flex-direction: row;
    align-items: center;
    border-bottom: 1px solid #f0f0f0;
    .item {
      flex: 1;
      display: flex;
      text-align: center;
      img {
        width: 100%;
      }
    }
    .item-btn {
      margin-right: 1em;
      font-size: 12px;
    }
  }
  .table-header {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    font-weight: bold;
  }
}
</style>
