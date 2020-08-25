<template>
  <div class="column">
    <h2 class="is-size-4">Bookmark</h2>
    <p>{{ bookmark }}</p>
    <p>{{ url }}</p>
    <p>{{ columnName }}</p>
  </div>
</template>

<script>
export default {
  name: "Column",
  props: {
    bookmark: String,
    url: String,
    columnName: String,
  },
  data: function() {
    return {};
  },
};
</script>

<style scoped>
.bookmark {
  background-color: #6272a4;
  border: 5px solid #44475a;
  border-radius: 5px;
  margin: 5px 0;
  padding: 5px;
}
</style>
