<template>
  <nav class="filter-nav">
    <button @click="updateFilter('all')" :class="{ active: status === 'all' }">
      View All
    </button>
    <button
      @click="updateFilter('completed')"
      :class="{ active: status === 'completed' }"
    >
      Completed
    </button>
    <button
      @click="updateFilter('ongoing')"
      :class="{ active: status === 'ongoing' }"
    >
      Ongoing
    </button>
  </nav>
</template>

<script>
export default {
  props: {
    status: String,
  },
  methods: {
    updateFilter(status) {
      this.$emit("filterProject", status);
    },
  },
};
</script>

<style lang="scss" scoped>
.filter-nav {
  text-align: left;
  button {
    background: none;
    border: none;
    color: #bbb;
    outline: none;
    font-size: 12px;
    text-transform: uppercase;
    margin-right: 10px;
    letter-spacing: 1px;
    font-weight: bold;
    cursor: pointer;
    &.active {
      color: #555;
    }
  }
}
</style>