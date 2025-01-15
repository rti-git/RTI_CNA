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
