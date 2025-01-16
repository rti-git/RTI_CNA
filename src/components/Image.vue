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
      images: [],
    };
  },

  computed: {
    totalPages() {
      return Math.ceil(this.filteredImages.length / this.itemsPerPage);
    },
    filteredImages() {
      return this.images.filter((image) => {
        const matchesSearchTerm = image.name
          .toLowerCase()
          .includes(this.searchTerm.toLowerCase());
        const matchesSource =
          this.selectedSources.all ||
          this.selectedSources[image.from.toLowerCase()];
        return matchesSearchTerm && matchesSource;
      });
    },
    paginatedData() {
      const start = (this.currentPage - 1) * this.itemsPerPage;
      const end = start + this.itemsPerPage;
      return this.filteredImages.slice(start, end);
    },
  },

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
