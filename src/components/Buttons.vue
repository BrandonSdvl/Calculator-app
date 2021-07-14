<template lang="pug">
main.buttons
  button.buttons__item(v-on:click="addNumber('7')") 7
  button.buttons__item(v-on:click="addNumber('8')") 8
  button.buttons__item(v-on:click="addNumber('9')") 9
  button.buttons__item.buttons__item--blue(v-on:click="del()") DEL
  button.buttons__item(v-on:click="addNumber('4')") 4
  button.buttons__item(v-on:click="addNumber('5')") 5
  button.buttons__item(v-on:click="addNumber('6')") 6
  button.buttons__item(v-on:click="addOperation('+')") +
  button.buttons__item(v-on:click="addNumber('1')") 1
  button.buttons__item(v-on:click="addNumber('2')") 2
  button.buttons__item(v-on:click="addNumber('3')") 3
  button.buttons__item(v-on:click="addOperation('-')") -
  button.buttons__item(v-on:click="dot()") .
  button.buttons__item(v-on:click="addNumber('0')") 0
  button.buttons__item(v-on:click="addOperation('/')") /
  button.buttons__item(v-on:click="addOperation('*')") x
  button.buttons__item.buttons__item--blue(v-on:click="reset()") RESET
  button.buttons__item.buttons__item--red(v-on:click="result()") =
</template>
<script>
export default {
  name: "Buttons",
  props: {
    content: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      operations: [],
    };
  },
  methods: {
    async addOperation(operation) {
      await this.checkInvalid();
      if (this.content.length == 0 && operation != "-") {
        return;
      }

      if (this.content.length == 1 && this.content == "-") {
        if (operation == "-") {
          return;
        }
        this.reset();
        return;
      }

      if (
        (this.content.substr(this.content.length - 1) == "*" ||
          this.content.substr(this.content.length - 1) == "/") &&
        operation == "-"
      ) {
        this.$emit("update:content", `${this.content}${operation}`);
        this.operations.push(operation);
        return;
      } else if (
        this.content.substr(this.content.length - 1) == "+" ||
        this.content.substr(this.content.length - 1) == "-" ||
        this.content.substr(this.content.length - 1) == "*" ||
        this.content.substr(this.content.length - 1) == "/"
      ) {
        await this.del();
        this.addOperation(operation);
        return;
      }

      if (this.content == "." || this.content == "-.") {
        this.reset();
        return;
      } else if (
        this.content.substr(this.content.length - 2) == "+." ||
        this.content.substr(this.content.length - 2) == "-." ||
        this.content.substr(this.content.length - 2) == "*." ||
        this.content.substr(this.content.length - 2) == "/."
      ) {
        await this.del();
        await this.del();
        this.addOperation(operation);
        return;
      }
      this.$emit("update:content", `${this.content}${operation}`);
      this.operations.push(operation);
    },
    async addNumber(number) {
      if (number == "0") {
        if (
          this.content.substr(this.content.length - 2) == "+0" ||
          this.content.substr(this.content.length - 2) == "-0" ||
          this.content.substr(this.content.length - 2) == "*0" ||
          this.content.substr(this.content.length - 2) == "/0" ||
          this.content == "0"
        ) {
          return;
        }
      } else {
        if (
          this.content.substr(this.content.length - 2) == "+0" ||
          this.content.substr(this.content.length - 2) == "-0" ||
          this.content.substr(this.content.length - 2) == "*0" ||
          this.content.substr(this.content.length - 2) == "/0" ||
          this.content == "0"
        ) {
          await this.del();
        }
      }
      await this.checkInvalid();
      this.$emit("update:content", `${this.content}${number}`);
    },
    async del() {
      await this.checkInvalid();

      let last = this.content.substring(
        this.content.length - 1,
        this.content.length
      );
      if (last == "+" || last == "-" || last == "/" || last == "*") {
        this.operations.pop();
      }
      this.$emit(
        "update:content",
        this.content.substring(0, this.content.length - 1)
      );
    },
    dot() {
      let figures = this.content.split(
        this.operations[this.operations.length - 1]
      );
      if (figures[figures.length - 1].includes(".")) {
        return;
      } else {
        this.$emit("update:content", `${this.content}.`);
      }
    },
    reset() {
      this.$emit("update:content", "");
      this.operations = [];
    },
    async result() {
      if (
        this.content.substr(this.content.length - 1) == "." ||
        this.content.substr(this.content.length - 1) == "-"
      ) {
        await this.del();
        this.result();
        return;
      }

      if (this.content.length == 0) {
        return;
      }

      if (
        this.content.substr(this.content.length - 1) == "+" ||
        this.content.substr(this.content.length - 1) == "-" ||
        this.content.substr(this.content.length - 1) == "*" ||
        this.content.substr(this.content.length - 1) == "/"
      ) {
        await this.del();
      }
      this.$emit("update:content", eval(this.content).toString());
      this.operations = [];
    },
    checkInvalid() {
      if (
        this.content.toLowerCase() == "nan" ||
        this.content.toLowerCase() == "infinity" ||
        this.content.toLowerCase() == "-infinity" ||
        this.content.toLowerCase() == "error"
      ) {
        this.reset();
      }
    },
  },
};
</script>