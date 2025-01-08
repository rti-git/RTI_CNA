<template>
  <div class="image-list">
    <h2>RTI新聞圖片</h2>
    <div class="title">
      <input v-model="searchTerm" class="int" placeholder="搜尋圖片名稱" />
      <div class="label-grid">
        <div class="col">
          <div class="label">
            <input
              type="checkbox"
              v-model="selectedSources.all"
              @change="toggleSelectAll"
            />
            <label for="first-col">全選</label>
          </div>
        </div>
        <div class="col">
          <div class="label">
            <input
              type="checkbox"
              v-model="selectedSources.rti"
              :disabled="selectedSources.all"
            />
            <label for="first-col">Rti央廣</label>
          </div>
          <div class="label">
            <input
              type="checkbox"
              v-model="selectedSources.cna"
              :disabled="selectedSources.all"
            />
            <label for="first-col">CNA中央社</label>
          </div>
          <div class="label">
            <input
              type="checkbox"
              v-model="selectedSources.ap"
              :disabled="selectedSources.all"
            />
            AP美聯社
          </div>
          <div class="label">
            <input
              type="checkbox"
              v-model="selectedSources.afp"
              :disabled="selectedSources.all"
            />
            AFP法新社
          </div>
        </div>
        <div class="col">
          <div class="label">
            <input
              type="checkbox"
              v-model="selectedSources.reuters"
              :disabled="selectedSources.all"
            />
            路透社
          </div>
          <div class="label">
            <input
              type="checkbox"
              v-model="selectedSources.others"
              :disabled="selectedSources.all"
            />
            其他
          </div>
          <div class="label">
            <input
              type="checkbox"
              v-model="selectedSources.program"
              :disabled="selectedSources.all"
            />
            節目
          </div>
          <div class="label">
            <input
              type="checkbox"
              v-model="selectedSources.company"
              :disabled="selectedSources.all"
            />
            公企
          </div>
        </div>
      </div>
    </div>
  </div>
  <div v-if="result" class="image-list">
    <div class="table-header">
      <div class="header-item">編號</div>
      <div class="header-item">圖片</div>
      <div class="header-item">說明</div>
      <div class="header-item">建立時間</div>
      <div class="header-item">圖片來源</div>
      <div class="header-item">功能</div>
    </div>
    <div class="image" v-for="image in filteredImages" :key="image.id">
      <div class="item">{{ image.id }}</div>
      <div class="item"><img :src="image.url" :alt="image.name" /></div>
      <div class="item">{{ image.content }}</div>
      <div class="item">{{ image.date }}</div>
      <div class="item">{{ image.from }}</div>
      <div class="item">
        <button @click="someFunction(image.id)">預覽</button>
        <button @click="someFunction(image.id)">下載</button>
        <button @click="someFunction(image.id)">複製</button>
      </div>
    </div>
    <div v-if="searchTerm">
      <p>沒有找到相關圖片。</p>
    </div>
  </div>
</template>

<script>
import { ref, computed } from "vue";
import { images } from "../../server/api/api";

export default {
  setup() {
    const searchTerm = ref("");
    const selectedSources = ref({
      all: false,
      rti: false,
      cna: false,
      ap: false,
      afp: false,
      reuters: false,
      others: false,
      program: false,
      company: false,
    });
    const filteredImages = computed(() => {
      return images
        .filter((image) => {
          const matchesSearchTerm = image.name
            .toLowerCase()
            .includes(searchTerm.value.toLowerCase());
          const matchesSource =
            selectedSources.value.all ||
            (selectedSources.value.rti && image.from === "RTI") ||
            (selectedSources.value.cna && image.from === "CNA") ||
            (selectedSources.value.ap && image.from === "AP") ||
            (selectedSources.value.afp && image.from === "AFP") ||
            (selectedSources.value.program && image.from === "PROGRAM") ||
            (selectedSources.value.company && image.from === "COMPANY") ||
            (selectedSources.value.reuters && image.from === "REUTERS") ||
            (selectedSources.value.others && image.from === "OTHERS");
          return matchesSearchTerm && matchesSource;
        })
        .slice(0, 5);
    });
    const result = computed(() => filteredImages.value.length > 0);

    const toggleSelectAll = () => {
      selectedSources.value.rti = selectedSources.value.all;
      selectedSources.value.cna = selectedSources.value.all;
      selectedSources.value.ap = selectedSources.value.all;
      selectedSources.value.afp = selectedSources.value.all;
      selectedSources.value.program = selectedSources.value.all;
      selectedSources.value.reuters = selectedSources.value.all;
      selectedSources.value.others = selectedSources.value.all;
      selectedSources.value.company = selectedSources.value.all;
    };

    return {
      searchTerm,
      filteredImages,
      result,
      toggleSelectAll,
      selectedSources,
    };
  },
};
</script>

<style lang="scss">
.image-list {
  display: flex;
  align-items: center;
  flex-direction: column;
  flex-wrap: wrap;
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
  .table-header {
    display: flex;
    align-items: center;
    font-weight: bold;
    background-color: #f0f0f0;
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
// .image {
//   display: flex;
//   align-items: center;
// }
</style>
