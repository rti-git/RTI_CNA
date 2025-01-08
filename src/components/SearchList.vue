<template>
  <div style="display: flex; align-items: center; margin-bottom: 20px">
    <input
      :value="searchTerm"
      @input="$emit('update:searchTerm', $event.target.value)"
      placeholder="搜尋圖片名稱"
    />
    <label style="margin-left: 10px">
      <input
        type="checkbox"
        :checked="selectedSources.all"
        @change="
          $emit('update:selectedSources', {
            ...selectedSources,
            all: !selectedSources.all,
          })
        "
      />
      全選
    </label>
    <label style="margin-left: 10px">
      <input
        type="checkbox"
        :checked="selectedSources.central"
        :disabled="selectedSources.all"
        @change="
          $emit('update:selectedSources', {
            ...selectedSources,
            central: !selectedSources.central,
          })
        "
      />
      中央社
    </label>
    <label style="margin-left: 10px">
      <input
        type="checkbox"
        :checked="selectedSources.reuters"
        :disabled="selectedSources.all"
        @change="
          $emit('update:selectedSources', {
            ...selectedSources,
            reuters: !selectedSources.reuters,
          })
        "
      />
      路透社
    </label>
    <label style="margin-left: 10px">
      <input
        type="checkbox"
        :checked="selectedSources.others"
        :disabled="selectedSources.all"
        @change="
          $emit('update:selectedSources', {
            ...selectedSources,
            others: !selectedSources.others,
          })
        "
      />
      其他
    </label>
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
};
</script>
