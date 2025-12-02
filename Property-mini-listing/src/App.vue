<template>
  <div class="app-container">

    <HeaderBar :activeCount="activeListingsCount" />

    <SearchSortBar
      :modelValue="searchQuery"
      :sortProp="sortOrder"
      :showUnavailableProp="showUnavailable"
      @update:search="searchQuery = $event"
      @update:sort="sortOrder = $event"
      @update:showUnavailable="showUnavailable = $event"
    />

    <main>
      <div v-if="filtered.length === 0" class="empty">
        No properties match your search
      </div>

      <section class="grid">
        <PropertyCard
          v-for="p in sorted"
          :key="p.id"
          :property="p"
          :favs="favs"
          @toggle-fav="onToggleFav"
        />
      </section>
    </main>

  </div>
</template>

<script>
import HeaderBar from './components/HeaderBar.vue'
import SearchSortBar from './components/SearchSortBar.vue'
import PropertyCard from './components/PropertyCard.vue'
import { properties as initial } from './data/properties.js'


export default {
name: 'App',
components: { HeaderBar, SearchSortBar, PropertyCard },
data() {
return {
raw: initial,
searchQuery: '',
sortOrder: 'low',
showUnavailable: false,
favs: this.loadFavs()
}
},
computed: {
filtered() {
const q = this.searchQuery.trim().toLowerCase()
return this.raw.filter(p => {
if (!this.showUnavailable && !p.available) return false
if (!q) return true
return p.title.toLowerCase().includes(q) || p.location.toLowerCase().includes(q)
})
},
sorted() {
return [...this.filtered].sort((a,b) => {
return this.sortOrder === 'low' ? a.price - b.price : b.price - a.price
})
},
activeListingsCount() {
return this.raw.filter(p => p.available).length
}
},
methods: {
onToggleFav(id) {
const i = this.favs.indexOf(id)
if (i === -1) this.favs.push(id)
else this.favs.splice(i,1)
this.saveFavs()
},
loadFavs() {

// eslint-disable-next-line no-unused-vars
try { return JSON.parse(localStorage.getItem('hb.favs') || '[]') } catch(error) {return []}
},
saveFavs() {

// eslint-disable-next-line no-unused-vars
try { localStorage.setItem('hb.favs', JSON.stringify(this.favs)) } catch (error) {return []}
}
}
}
</script>

<style>

* {
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
}

body {
  font-family: "Inter", Arial, sans-serif;
  background: lightskyblue;
  color: purple;
  line-height: 1.5;
}

.app-container {
  max-width: 1200px;
  margin: auto;
  padding: 20px;
}

.empty {
  text-align: center;
  padding: 40px 0;
  font-size: 18px;
  color: purple;
  font-style: italic;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 22px;
}

.badge {
  display: inline-block;
  background: lightblue;
  color: purple;
  padding: 3px 8px;
  border-radius: 6px;
  font-size: 12px;
  margin-bottom: 8px;
}

.badge.unavailable {
  background: purple;
}

</style>
