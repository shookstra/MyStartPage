<template>
  <div id="app" class="columns">
    <StartPage class="column" />
  </div>
</template>

<script>
import StartPage from "./components/StartPage.vue";

export default {
  name: "App",
  components: {
    StartPage
  }
};
</script>

<style lang="scss">
@import "~bulma/sass/utilities/_all";
@import url("https://fonts.googleapis.com/css2?family=Nunito&display=swap");

$family-sans-serif: "Nunito", sans-serif;

$primary: #bd93f9;
$primary-invert: findColorInvert($primary);
// $link: #ff79c6;
// $link-invert: findColorInvert($link);
$dark-grey: #282a36;
$light-grey: #44475a;
$light-grey-invert: findColorInvert($light-grey);

// set colors
$colors: (
  "primary": (
    $primary,
    $primary-invert
  ),
  // "link": (
  //   $link,
  //   $link-invert
  // ),
  "light-grey": (
    $light-grey,
    $light-grey-invert
  )
);

$link: $link;

* {
  font-family: $family-sans-serif;
}

#app {
  color: white;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-content: center;
}

html {
  background-color: $dark-grey;
}

body {
  background-color: $dark-grey;
  min-height: 100vh;
}
</style>
