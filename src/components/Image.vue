<template>
  <SearchList
    :searchTerm="searchTerm"
    :selectedSources="selectedSources"
    @search-term-change="updateSearchTerm"
    @source-change="updateSources"
  />

  <ImageList
    :images="paginatedData"
    :searchTerm="searchTerm"
    :selectedSources="selectedSources"
    :itemsPerPage="itemsPerPage"
    :currentPage="currentPage"
    @preview="previewImage"
  />

  <Pagination
    :current-page="currentPage"
    :total-pages="totalPages"
    @update:currentPage="updateCurrentPage"
  />

  <!-- 預覽模態框 -->
  <div v-if="isPreviewVisible" class="modal">
    <div class="modal-content">
      <span class="close" @click="isPreviewVisible = false">&times;</span>
      <img :src="previewImageUrl" alt="預覽" />
    </div>
  </div>
</template>

<script>
import { ref, computed, onMounted } from "vue";
import SearchList from "./SearchList.vue";
import Pagination from "./Pagination.vue";
import ImageList from "./ImageList.vue";

export default {
  components: {
    SearchList,
    ImageList,
    Pagination,
  },
  data() {
    return {
      isPreviewVisible: false,
      previewImageUrl: "",
      currentPage: 1,
      itemsPerPage: 5,
      searchTerm: "",
      selectedSources: {
        all: false,
        rti: false,
        cna: false,
        ap: false,
        afp: false,
        reuters: false,
        others: false,
        program: false,
        company: false,
      },
      images: [], // 假設這裡是你的圖片數據
    };
  },

  computed: {
    totalPages() {
      return Math.ceil(this.filteredImages.length / this.itemsPerPage);
    },
    filteredImages() {
      // 過濾資料根據搜尋條件
      return this.images.filter((image) => {
        return image.name.toLowerCase().includes(this.searchTerm.toLowerCase());
      });
    },
    paginatedData() {
      const start = (this.currentPage - 1) * this.itemsPerPage;
      const end = start + this.itemsPerPage;
      return this.filteredImages.slice(start, end);
    },
  },

  // watch: {
  //   filteredImages(newData) {
  //     if (newData.length === 0) return;
  //     const targetPage = Math.ceil(newData.length / this.itemsPerPage);

  //     if (this.currentPage > targetPage) {
  //       this.currentPage = targetPage;
  //     }
  //   },
  // },

  mounted() {
    this.fetchImages();
  },

  methods: {
    async fetchImages() {
      const response = await fetch("/images.json");
      const data = await response.json();
      this.images = data;
    },
    updateCurrentPage(page) {
      this.currentPage = page;
    },
    previewImage(url) {
      this.previewImageUrl = url;
      this.isPreviewVisible = true;
    },
    updateSearchTerm(newTerm) {
      this.searchTerm = newTerm;
      this.currentPage = 1;
    },
    updateSources(newSources) {
      this.selectedSources = newSources;
      this.currentPage = 1;
    },
  },
};
</script>

<style lang="scss">
.image-list {
  display: flex;
  align-items: center;
  flex-direction: row;
  flex-wrap: nowrap;
  .image {
    display: flex;
    flex: 0 1 30%;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
    padding: 10px;
    text-align: center;
    width: 100%;
    img {
      max-width: 100%;
      height: auto;
    }
  }

  h2 {
    width: 50%;
    margin: 0;
  }
  input {
    width: 100%;
    padding: 0.5rem;
    flex: 2;
    .title {
      display: flex;
      align-items: center;
      margin-bottom: 20px;
    }
  }
}
.label {
  display: flex;
  flex-direction: row;
  // vertical-align: text-top;
}
.label-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1rem;
  height: 100px;
  width: 800px;
  flex: 3;
}

.label:nth-child(1) {
  grid-column: span 4;
}

.label:nth-child(2),
.label:nth-child(3),
.label:nth-child(4),
.label:nth-child(5) {
  grid-row: 2;
}

.label:nth-child(6),
.label:nth-child(7),
.label:nth-child(8),
.label:nth-child(9) {
  grid-row: 3;
}

.title {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-direction: row;
  .int {
    height: 10px;
  }
}
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

.modal {
  display: flex;
  position: fixed;
  z-index: 1000;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7);
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: #fa0;
  padding: 20px;
  border-radius: 5px;
  position: relative;
}

.close {
  position: absolute;
  top: 10px;
  right: 15px;
  cursor: pointer;
  font-size: 100px;
}
</style>
