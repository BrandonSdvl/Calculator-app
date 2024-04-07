<script setup>
import { ref } from 'vue'
import { themes } from '../themes/themes'

const props = defineProps({
  content: {
    type: String,
    default: "",
  },
})

const savedTheme = localStorage.getItem("theme");

const initialTheme = (savedTheme && themes.map(theme => theme.name).includes(JSON.parse(savedTheme).theme)) 
  ? JSON.parse(savedTheme).theme 
  : null;

const userPrefersLight =
  (window.matchMedia &&
    window.matchMedia("(prefers-color-scheme: light)").matches) ? "theme2" : null;
    
const picked = ref(initialTheme || userPrefersLight || "theme1");

const saveTheme = () => {
  localStorage.setItem("theme", JSON.stringify({ theme: picked.value }));

  const currentTheme = themes.find(theme => theme.name === picked.value);
  const styles = document.documentElement.style;
  const customStyles = Object.keys(currentTheme.styles);

  for (const style of customStyles) {
    styles.setProperty(style, currentTheme.styles[style]);
  }
}

saveTheme()

const switchClicked = () => {
  const currentIndex = themes.findIndex(theme => theme.name === picked.value);
  picked.value = themes[(currentIndex + 1) % themes.length].name;
  saveTheme()
}

</script>

<template lang="pug">
.top
  header.header
    h1.header__title calc
    .header__switch
      h2.header__subtitle Theme
      .switch-container
        .label-container
          label.label-container__item(for="theme1") 1
          label.label-container__item(for="theme2") 2
          label.label-container__item(for="theme3") 3
        #switch.switch(
          :class="`${picked}-selected`",
          v-on:click="switchClicked()"
        )
          input#theme1.switch__item(
            type="radio",
            name="theme",
            v-model="picked",
            value="theme1"
          )
          input#theme2.switch__item(
            type="radio",
            name="theme",
            v-model="picked",
            value="theme2"
          )
          input#theme3.switch__item(
            type="radio",
            name="theme",
            v-model="picked",
            value="theme3"
          )
  .screen(role="region") {{ content }}
</template>
