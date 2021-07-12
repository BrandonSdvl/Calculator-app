<template lang="pug">
main.buttons(role="main")
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
  data() {
    return {
      operations: [],
    };
  },
  methods: {
    addOperation(operation) {
      this.checkInvalid();
      if (this.$parent.content.length == 0 && operation != "-") {
        return;
      }

      if (this.$parent.content.length == 1 && this.$parent.content == "-") {
        if (operation == "-") {
          return;
        }
        this.reset();
        return;
      }

      if (
        (this.$parent.content.substr(this.$parent.content.length - 1) == "*" ||
          this.$parent.content.substr(this.$parent.content.length - 1) ==
            "/") &&
        operation == "-"
      ) {
        this.$parent.content += operation;
        this.operations.push(operation);
        return;
      } else if (
        this.$parent.content.substr(this.$parent.content.length - 1) == "+" ||
        this.$parent.content.substr(this.$parent.content.length - 1) == "-" ||
        this.$parent.content.substr(this.$parent.content.length - 1) == "*" ||
        this.$parent.content.substr(this.$parent.content.length - 1) == "/"
      ) {
        this.del();
        this.addOperation(operation);
        return;
      }

      if (this.$parent.content == "." || this.$parent.content == "-.") {
        this.reset();
        return;
      } else if (
        this.$parent.content.substr(this.$parent.content.length - 2) == "+." ||
        this.$parent.content.substr(this.$parent.content.length - 2) == "-." ||
        this.$parent.content.substr(this.$parent.content.length - 2) == "*." ||
        this.$parent.content.substr(this.$parent.content.length - 2) == "/."
      ) {
        this.del();
        this.del();
        this.addOperation(operation);
        return;
      }

      this.$parent.content += operation;
      this.operations.push(operation);
    },
    addNumber(number) {
      if (number == "0") {
        if (
          this.$parent.content.substr(this.$parent.content.length - 2) ==
            "+0" ||
          this.$parent.content.substr(this.$parent.content.length - 2) ==
            "-0" ||
          this.$parent.content.substr(this.$parent.content.length - 2) ==
            "*0" ||
          this.$parent.content.substr(this.$parent.content.length - 2) ==
            "/0" ||
          this.$parent.content == "0"
        ) {
          return;
        }
      } else {
        if (
          this.$parent.content.substr(this.$parent.content.length - 2) ==
            "+0" ||
          this.$parent.content.substr(this.$parent.content.length - 2) ==
            "-0" ||
          this.$parent.content.substr(this.$parent.content.length - 2) ==
            "*0" ||
          this.$parent.content.substr(this.$parent.content.length - 2) ==
            "/0" ||
          this.$parent.content == "0"
        ) {
          this.del();
        }
      }
      this.checkInvalid();
      this.$parent.content += number;
    },
    del() {
      this.checkInvalid();

      let last = this.$parent.content.substring(
        this.$parent.content.length - 1,
        this.$parent.content.length
      );
      if (last == "+" || last == "-" || last == "/" || last == "*") {
        this.operations.pop();
      }
      this.$parent.content = this.$parent.content.substring(
        0,
        this.$parent.content.length - 1
      );
    },
    dot() {
      let figures = this.$parent.content.split(
        this.operations[this.operations.length - 1]
      );
      if (figures[figures.length - 1].includes(".")) {
        return;
      } else {
        this.$parent.content += ".";
      }
    },
    reset() {
      this.$parent.content = "";
      this.operations = [];
    },
    result() {
      if (
        this.$parent.content.substr(this.$parent.content.length - 1) == "." ||
        this.$parent.content.substr(this.$parent.content.length - 1) == "-"
      ) {
        this.del();
        this.result();
        return;
      }

      if (this.$parent.content.length == 0) {
        return;
      }

      if (
        this.$parent.content.substr(this.$parent.content.length - 1) == "+" ||
        this.$parent.content.substr(this.$parent.content.length - 1) == "-" ||
        this.$parent.content.substr(this.$parent.content.length - 1) == "*" ||
        this.$parent.content.substr(this.$parent.content.length - 1) == "/"
      ) {
        this.del();
      }
      this.$parent.content = eval(this.$parent.content).toString();
      this.operations = [];
    },
    checkInvalid() {
      if (
        this.$parent.content.toLowerCase() == "nan" ||
        this.$parent.content.toLowerCase() == "infinity" ||
        this.$parent.content.toLowerCase() == "error"
      ) {
        this.reset();
      }
    },
  },
};
</script>