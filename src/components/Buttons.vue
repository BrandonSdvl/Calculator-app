<template lang="pug">
.buttons
  button.buttons__item 7
  button.buttons__item 8
  button.buttons__item 9
  button.buttons__item.buttons__item--blue DEL
  button.buttons__item 4
  button.buttons__item 5
  button.buttons__item 6
  button.buttons__item +
  button.buttons__item 1
  button.buttons__item 2
  button.buttons__item 3
  button.buttons__item -
  button.buttons__item .
  button.buttons__item 0
  button.buttons__item /
  button.buttons__item x
  button.buttons__item.buttons__item--blue RESET
  button.buttons__item.buttons__item--red =
</template>
<script>
export default {
  name: "Buttons",
};
</script>