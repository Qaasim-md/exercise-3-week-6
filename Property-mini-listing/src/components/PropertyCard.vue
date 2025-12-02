<template>
  <div class="card">
    <div class="ribbon" style="position: relative">
      <img :src="property.image" :alt="property.title" />
      <div v-if="!property.available" class="badge">Not Available</div>
    </div>

    <div class="card-content">
      <div class="card-row">
        <div>
          <div style="font-weight: 700">{{ property.title }}</div>
          <div class="small">{{ property.location }} • {{ property.type }}</div>
        </div>
        <div style="text-align: right">
          <div class="price">R {{ formattedPrice }}</div>
          <button
            class="bookmark"
            @click="toggleFav"
            :title="isFav ? 'Remove bookmark' : 'Bookmark'"
          >
            <svg
              v-if="isFav"
              class="fav-active"
              xmlns="http://www.w3.org/2000/svg"
              width="20"
              height="20"
              viewBox="0 0 24 24"
              fill="currentColor"
            >
              <path
                d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 6 4 4 6.5 4c1.54 0 3.04.99 3.57 2.36h0c.53-1.37 2.03-2.36 3.57-2.36C18 4 20 6 20 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z"
              />
            </svg>
            <svg
              v-else
              xmlns="http://www.w3.org/2000/svg"
              width="20"
              height="20"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
            >
              <path
                d="M20.84 4.61a5.5 5.5 0 0 0-7.78 0L12 5.67l-1.06-1.06a5.5 5.5 0 0 0-7.78 7.78L12 21.23l8.84-8.84a5.5 5.5 0 0 0 0-7.78z"
              />
            </svg>
          </button>
        </div>
      </div>

      <div class="small">ID: {{ property.id }}</div>
    </div>
  </div>
</template>


<script>
export default {
  name: "PropertyCard",
  props: {
    property: { type: Object, required: true },
    favs: { type: Array, default: () => [] },
  },
  emits: ["toggle-fav"],
  computed: {
    formattedPrice() {
      return this.property.price.toLocaleString();
    },
    isFav() {
      return this.favs.includes(this.property.id);
    },
  },
  methods: {
    toggleFav() {
      this.$emit("toggle-fav", this.property.id);
    },
  },
};
</script>

<style>

.property-card {
  background: purple;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0,0,0,0.08);
  transition: transform .15s ease, box-shadow .15s ease;
}

.property-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 6px 18px rgba(0,0,0,0.12);
}

.property-card img {
  width: 50%;
  height: 150px;
  object-fit: cover;
}

.property-card .content {
  padding: 20px;
}

.property-card h3 {
  font-size: 18px;
  margin-bottom: 6px;
}

.property-card .location {
  font-size: 14px;
  color: #777;
  margin-bottom: 10px;
}

.property-card .price {
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 12px;
}

.property-card .fav-btn {
  background: none;
  border: none;
  font-size: 20px;
  cursor: pointer;
  transition: color .2s;
}

.property-card .fav-btn.active {
  color: purple;
}


</style>
