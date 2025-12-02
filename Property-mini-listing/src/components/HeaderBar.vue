<template>
  <header class="header">
    <div class="brand">
      <h1>Homes and Beyond</h1>
      <p>{{ activeCount }} properties available</p>
    </div>
    <div class="controls">
      <slot />
    </div>
  </header>
</template>


<script>
export default {
  name: "HeaderBar",
  props: {
    activeCount: { type: Number, default: 0 },
  },
};
</script>

<style>

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px 0;
}

header h1 {
  font-size: 50px;
  font-weight: 1000;
}

header .active-count {
  background: lightblue;
  padding: 6px 12px;
  color: purple;
  border-radius: 6px;
  font-size: 14px;
}

</style>
