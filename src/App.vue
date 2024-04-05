<script setup>
import Top from "./components/Top.vue";
import Buttons from "./components/Buttons.vue";
import { ref } from 'vue'

const content = ref("")

</script>

<template lang="pug">
#app
  Top(:content="content")
  Buttons(:content="content" @updateContent="(e) => content = e")
  footer.links
    a.links__item(href="http://github.com/brandonsdvl", target="_blank") BrandonSdvl
    span.links__separator @
    a.links__item(href="https://www.frontendmentor.io/", target="_blank") Frontend Mentor
</template>

<style lang="scss" src="./scss/styles.scss"></style>