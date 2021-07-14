<template lang="pug">
#app
  Top(:content="content")
  Buttons(:content.sync="content")
  footer.links
    a.links__item(href="http://github.com/brandonsdvl", target="_blank") BrandonSdvl
    span.links__separator @
    a.links__item(href="https://www.frontendmentor.io/", target="_blank") Frontend Mentor
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