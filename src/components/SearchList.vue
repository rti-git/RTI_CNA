<template>
  <div class="image-list">
    <h2>RTI新聞圖片</h2>
    <div class="title">
      <input
        v-model="localSearchTerm"
        @input="onSearchTermChange"
        class="int"
        placeholder="搜尋圖片名稱"
      />
      <div class="label-grid">
        <div class="col">
          <div class="label">
            <input
              type="checkbox"
              v-model="localSelectedSources.all"
              @change="onSelectAllChange"
            />
            <label for="first-col">全選</label>
          </div>
        </div>
        <div class="col">
          <div class="label">
            <input
              type="checkbox"
              v-model="localSelectedSources.rti"
              :disabled="localSelectedSources.all"
              @change="onSourcesChange"
            />
            <label for="first-col">Rti央廣</label>
          </div>
          <div class="label">
            <input
              type="checkbox"
              v-model="localSelectedSources.cna"
              :disabled="localSelectedSources.all"
              @change="onSourcesChange"
            />
            <label for="first-col">CNA中央社</label>
          </div>
          <div class="label">
            <input
              type="checkbox"
              v-model="localSelectedSources.ap"
              :disabled="localSelectedSources.all"
              @change="onSourcesChange"
            />
            AP美聯社
          </div>
          <div class="label">
            <input
              type="checkbox"
              v-model="localSelectedSources.afp"
              :disabled="localSelectedSources.all"
              @change="onSourcesChange"
            />
            AFP法新社
          </div>
        </div>
        <div class="col">
          <div class="label">
            <input
              type="checkbox"
              v-model="localSelectedSources.reuters"
              :disabled="localSelectedSources.all"
              @change="onSourcesChange"
            />
            路透社
          </div>
          <div class="label">
            <input
              type="checkbox"
              v-model="localSelectedSources.others"
              :disabled="localSelectedSources.all"
              @change="onSourcesChange"
            />
            其他
          </div>
          <div class="label">
            <input
              type="checkbox"
              v-model="localSelectedSources.program"
              :disabled="localSelectedSources.all"
              @change="onSourcesChange"
            />
            節目
          </div>
          <div class="label">
            <input
              type="checkbox"
              v-model="localSelectedSources.company"
              :disabled="localSelectedSources.all"
              @change="onSourcesChange"
            />
            公企
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    searchTerm: {
      type: String,
      required: true,
    },
    selectedSources: {
      type: Object,
      required: true,
    },
  },

  data() {
    return {
      localSearchTerm: this.searchTerm,
      localSelectedSources: { ...this.selectedSources },
    };
  },

  // watch: {
  //   searchTerm(newTerm) {
  //     this.localSearchTerm = newTerm;
  //   },
  //   selectedSources(newSources) {
  //     this.localSelectedSources = { ...newSources };
  //   },
  // },

  methods: {
    onSearchTermChange() {
      this.$emit("search-term-change", this.localSearchTerm);
    },
    onSourcesChange() {
      this.$emit("source-change", this.localSelectedSources);
    },
    onSelectAllChange() {
      const allSelected = this.localSelectedSources.all;
      Object.keys(this.localSelectedSources).forEach((key) => {
        if (key !== "all") {
          this.localSelectedSources[key] = allSelected;
        }
      });
      this.onSourcesChange();
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
</style>
