<script setup>
import { ref } from 'vue'

const props = defineProps({
  content: {
    type: String,
    default: "",
  },
})

const emit = defineEmits(["updateContent"])

const operations = ref([])

const addOperation = async (operation) => {
  await checkInvalid();
  if (props.content.length == 0 && operation != "-") {
    return;
  }

  if (props.content.length == 1 && props.content == "-") {
    if (operation == "-") {
      return;
    }
    reset();
    return;
  }

  if (
    (props.content.substr(props.content.length - 1) == "*" ||
      props.content.substr(props.content.length - 1) == "/") &&
    operation == "-"
  ) {
    emit("updateContent", `${props.content}${operation}`);
    operations.value.push(operation);
    return;
  } else if (
    props.content.substr(props.content.length - 1) == "+" ||
    props.content.substr(props.content.length - 1) == "-" ||
    props.content.substr(props.content.length - 1) == "*" ||
    props.content.substr(props.content.length - 1) == "/"
  ) {
    await del();
    addOperation(operation);
    return;
  }

  if (props.content == "." || props.content == "-.") {
    reset();
    return;
  } else if (
    props.content.substr(props.content.length - 2) == "+." ||
    props.content.substr(props.content.length - 2) == "-." ||
    props.content.substr(props.content.length - 2) == "*." ||
    props.content.substr(props.content.length - 2) == "/."
  ) {
    await del();
    await del();
    addOperation(operation);
    return;
  }
  emit("updateContent", `${props.content}${operation}`);
  operations.value.push(operation);
}

const addNumber = async (number) => {
  if (number == "0") {
    if (
      props.content.substr(props.content.length - 2) == "+0" ||
      props.content.substr(props.content.length - 2) == "-0" ||
      props.content.substr(props.content.length - 2) == "*0" ||
      props.content.substr(props.content.length - 2) == "/0" ||
      props.content == "0"
    ) {
      return;
    }
  } else {
    if (
      props.content.substr(props.content.length - 2) == "+0" ||
      props.content.substr(props.content.length - 2) == "-0" ||
      props.content.substr(props.content.length - 2) == "*0" ||
      props.content.substr(props.content.length - 2) == "/0" ||
      props.content == "0"
    ) {
      await del();
    }
  }
  await checkInvalid();
  emit("updateContent", `${props.content}${number}`);
}

const del = async () => {
  await checkInvalid();

  let last = props.content.substring(
    props.content.length - 1,
    props.content.length
  );
  if (last == "+" || last == "-" || last == "/" || last == "*") {
    operations.value.pop();
  }

  emit("updateContent", props.content.substring(0, props.content.length - 1));
}

const dot = () => {
  let figures = props.content.split(
    operations.value[operations.value.length - 1]
  );
  if (figures[figures.length - 1].includes(".")) {
    return;
  } else {
    emit("updateContent", `${props.content}.`);
  }
}

const reset = () => {
  emit("updateContent", "");
  operations.value = [];
}

const result = async () => {
  if (
    props.content.substr(props.content.length - 1) == "." ||
    props.content.substr(props.content.length - 1) == "-"
  ) {
    await del();
    result();
    return;
  }

  if (props.content.length == 0) {
    return;
  }

  if (
    props.content.substr(props.content.length - 1) == "+" ||
    props.content.substr(props.content.length - 1) == "-" ||
    props.content.substr(props.content.length - 1) == "*" ||
    props.content.substr(props.content.length - 1) == "/"
  ) {
    await del();
  }

  emit("updateContent", eval(props.content).toString());
  operations.value = [];
}

const checkInvalid = () => {
  if (
    props.content.toLowerCase() == "nan" ||
    props.content.toLowerCase() == "infinity" ||
    props.content.toLowerCase() == "-infinity" ||
    props.content.toLowerCase() == "error"
  ) {
    reset();
  }
}
</script>

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
