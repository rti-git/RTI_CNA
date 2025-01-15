<template>
  <div class="pagination">
    <button @click="prevPage" :disabled="currentPage === 1">上一頁</button>

    <div class="page-numbers">
      <button v-if="currentPage > 5" @click="goToPage(1)">1</button>

      <span v-if="currentPage > 4">...</span>

      <button
        v-for="page in visiblePages"
        :key="page"
        @click="goToPage(page)"
        :class="{ active: page === currentPage }"
      >
        {{ page }}
      </button>

      <span v-if="currentPage < totalPages - 5">...</span>

      <button v-if="currentPage < totalPages - 2" @click="goToPage(totalPages)">
        {{ totalPages }}
      </button>
    </div>

    <button @click="nextPage" :disabled="currentPage === totalPages">
      下一頁
    </button>
    <span>第 {{ currentPage }} 頁 / 總頁數 {{ totalPages }}</span>
  </div>
</template>

<script>
export default {
  props: {
    currentPage: {
      type: Number,
      required: true,
    },
    totalPages: {
      type: Number,
      required: true,
    },
  },
  computed: {
    visiblePages() {
      const pages = [];
      const range = 5;
      const start = Math.max(2, this.currentPage - range);
      const end = Math.min(this.totalPages - 1, this.currentPage + range);

      for (let i = start; i <= end; i++) {
        pages.push(i);
      }
      return pages;
    },
  },
  methods: {
    prevPage() {
      if (this.currentPage > 1) {
        this.$emit("update:currentPage", this.currentPage - 1);
      }
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.$emit("update:currentPage", this.currentPage + 1);
      }
    },
    goToPage(page) {
      this.$emit("update:currentPage", page);
    },
  },
};
</script>

<style scoped>
.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
}

.pagination button {
  margin: 0 10px;
  padding: 5px 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  background-color: #f0f0f0;
  cursor: pointer;
  color: #000;
}

.pagination button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}
</style>
