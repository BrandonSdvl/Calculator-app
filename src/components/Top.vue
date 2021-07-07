<template lang="pug">
.top
  header.header
    h1.header__title calc
    .switch-container
      h2.switch-container__title Theme
      form.form
        .label-container
          label.label-container__item(for="theme1") 1
          label.label-container__item(for="theme2") 2
          label.label-container__item(for="theme3") 3
        #switch.switch
          input#theme1.switch__item(type="radio", name="theme")
          input#theme2.switch__item(type="radio", name="theme")
          input#theme3.switch__item(type="radio", name="theme")
  .screen(v-text="this.$parent.content")
</template>

<script>
export default {
  data() {
    return {
      operation: "00,00",
    };
  },
  name: "Top",
};
</script>
