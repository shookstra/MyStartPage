<template>
  <div class="column">
    <div>
      <p class="heading is-size-3">{{ name }}</p>
      <ul id="links">
        <li v-for="link in links" :key="link.id">
          <a :href="link.link" target="_blank" class="link is-size-5">{{ link.name }}</a>
        </li>
      </ul>
    </div>
    <!-- <b-button
      rounded
      class="is-primary add-button"
      size="is-medium"
      icon-left="plus"
      @click="populateLinks"
    >Add Link</b-button>-->
  </div>
</template>

<script>
export default {
  name: "Column",
  props: {
    name: String,
    links: Array
  },
  methods: {
    populateLinks: function() {
      alert("add link");
    }
  }
};
</script>

<style scoped lang="scss">
.column {
  padding: 15px;
  position: relative;
  min-height: 325px;
  background-color: #44475a;
}

.add-button {
  width: 95%;
  position: absolute;
  bottom: 0;
  left: 0;
  margin: 25px 10px 10px 10px;
  background-color: #44475a;
}

.heading {
  text-transform: capitalize;
  margin-bottom: 20px;
}

.link {
  // link hover trick from:
  // https://css-tricks.com/4-ways-to-animate-the-color-of-a-text-link-on-hover/
  position: relative;
  display: inline-block;
  font-size: 2em;
  font-weight: 800;
  color: #ff79c6;
  overflow: hidden;

  background: linear-gradient(to right, #8be9fd, #8be9fd 50%, #ff79c6 50%);

  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-size: 200% 100%;
  background-position: 100%;

  transition: background-position 275ms ease, border-left 275ms ease;

  padding-left: 5px;
  border-left: 2px solid transparent;
}

.link:hover {
  background-position: 0 100%;
  border-left: 2px solid #8be9fd;
}
</style>
