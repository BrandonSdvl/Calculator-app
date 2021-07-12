<template lang="pug">
.top
  header.header(role="banner") 
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
  .screen(role="region", v-text="this.$parent.content")
</template>

<script>
export default {
  data() {
    return {
      picked: "theme1",
      currentTheme: {},
      theme: [],
      theme1: {
        "--main-background": "hsl(222, 26%, 31%)",
        "--second-background": "hsl(223, 31%, 20%)",
        "--screen-background": "hsl(224, 36%, 15%)",

        // Text
        "--main-font-color": "hsl(221, 14%, 31%)",
        "--second-font-color": "#FCFEFD",
        "--third-font-color": "#FCFEFD",
        "--top-font-color": "#FCFEFD",

        // Keys
        "--key-background": "hsl(30, 25%, 89%)",
        "--key-shadow": "hsl(28, 16%, 65%)",
        "--second-key-background": "hsl(225, 21%, 49%)",
        "--second-key-shadow": "hsl(224, 28%, 35%)",
        "--emphasis-background": "hsl(6, 63%, 50%)",
        "--emphasis-shadow": "hsl(6, 70%, 34%)",
      },
      theme2: {
        "--main-background": "hsl(0, 0%, 90%)",
        "--second-background": "hsl(0, 5%, 81%)",
        "--screen-background": "hsl(0, 0%, 93%)",

        // Text
        "--main-font-color": "hsl(60, 10%, 19%)",
        "--second-font-color": "#FCFEFD",
        "--third-font-color": "#FCFEFD",
        "--top-font-color": "hsl(60, 10%, 19%)",

        // Keys
        "--key-background": "hsl(45, 7%, 89%)",
        "--key-shadow": "hsl(35, 11%, 61%)",
        "--second-key-background": "hsl(185, 42%, 37%)",
        "--second-key-shadow": "hsl(185, 58%, 25%)",
        "--emphasis-background": "hsl(25, 98%, 40%)",
        "--emphasis-shadow": "hsl(25, 99%, 27%)",
      },
      theme3: {
        "--main-background": "hsl(268, 75%, 9%)",
        "--second-background": "hsl(268, 71%, 12%)",
        "--screen-background": "hsl(268, 71%, 12%)",

        // Text
        "--main-font-color": "hsl(52, 100%, 62%)",
        "--second-font-color": "#FCFEFD",
        "--third-font-color": "hsl(198, 20%, 13%)",
        "--top-font-color": "hsl(52, 100%, 62%)",

        // Keys
        "--key-background": "hsl(268, 47%, 21%)",
        "--key-shadow": "hsl(285, 91%, 52%)",
        "--second-key-background": "#480773",
        "--second-key-shadow": "#BF16F5",
        "--emphasis-background": "hsl(176, 100%, 44%)",
        "--emphasis-shadow": "hsl(177, 92%, 70%)",
      },
    };
  },
  watch: {
    picked: function () {
      if (this.picked == "theme1") {
        this.currentTheme = this.theme1;
      } else if (this.picked == "theme2") {
        this.currentTheme = this.theme2;
      } else if (this.picked == "theme3") {
        this.currentTheme = this.theme3;
      }

      const styles = document.documentElement.style;
      const customStyles = Object.keys(this.currentTheme);
      for (const style of customStyles) {
        styles.setProperty(style, this.currentTheme[style]);
      }

      this.saveTheme();
    },
  },
  methods: {
    switchClicked() {
      if (this.picked == "theme1") {
        this.picked = "theme2";
      } else if (this.picked == "theme2") {
        this.picked = "theme3";
      } else if (this.picked == "theme3") {
        this.picked = "theme1";
      }
    },
    saveTheme() {
      this.theme = { theme: this.picked };
      localStorage.setItem("theme", JSON.stringify(this.theme));
    },
  },
  created() {
    if (localStorage.getItem("theme")) {
      this.theme = JSON.parse(localStorage.getItem("theme"));
      this.picked = this.theme.theme;
    } else {
      const userPrefersLight =
        window.matchMedia &&
        window.matchMedia("(prefers-color-scheme: light)").matches;
      if (userPrefersLight) {
        this.picked = "theme2";
      } else {
        this.picked = "theme1";
      }
      this.saveTheme();
    }
  },
  name: "Top",
};
</script>
