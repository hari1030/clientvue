<template>
  <div>
    <h4 v-for="(message, ind) in messagelist" v-bind:key="ind">
      {{ message }}
    </h4>
  </div>
</template>

<script>
export default {
  name: "Messagelist",
  props: ["messagelist"],
};
</script>

<style scoped></style>
