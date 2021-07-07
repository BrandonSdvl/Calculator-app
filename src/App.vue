<template lang="pug">
#app
  Top
  Buttons
</template>

<script>
import Top from "./components/Top";
import Buttons from "./components/Buttons";

export default {
  name: "App",
  components: {
    Top,
    Buttons,
  },
  data() {
    return {
      content: "",
    };
  },
};
</script>

<style lang="scss" src="./scss/styles.scss"></style>