<template>
  <div style="display: flex; gap: 10px; align-items: center">
    <input
      class="search-input"
      placeholder="Search by title or location..."
      v-model="search"
      @input="update"
    />

    <select class="select" v-model="sortOrder" @change="update">
      <option value="low">Price: low → high</option>
      <option value="high">Price: high → low</option>
    </select>

    <button class="select" @click="toggleShowUnavailable">
      {{ showUnavailable ? "Hide" : "Show" }} unavailable
    </button>
  </div>
</template>


<script>
export default {
  name: "SearchSortBar",
  props: {
    modelValue: String,
    sortProp: String,
    showUnavailableProp: Boolean,
  },
  emits: ["update:search", "update:sort", "update:showUnavailable"],
  data() {
    return {
      search: this.modelValue || "",
      sortOrder: this.sortProp || "low",
      showUnavailable: !!this.showUnavailableProp,
    };
  },
  methods: {
    update() {
      this.$emit("update:search", this.search);
      this.$emit("update:sort", this.sortOrder);
      this.$emit("update:showUnavailable", this.showUnavailable);
    },
    toggleShowUnavailable() {
      this.showUnavailable = !this.showUnavailable;
      this.update();
    },
  },
};
</script>

<style>

.search-sort-bar {
  margin: 20px 0 25px;
  display: flex;
  gap: 15px;
  flex-wrap: wrap;
}

.search-sort-bar input,
.search-sort-bar select {
  padding: 10px 14px;
  border: 1px solid purple;
  border-radius: 6px;
  flex: 1;
  font-size: 15px;
  background: lightblue
}

.search-sort-bar label {
  display: flex;
  align-items: center;
  gap: 6px;
  cursor: pointer;
}


</style>
