<template>
  <button :type="type" class="btn">{{ txt }}</button>
</template>

<script>
export default {
  name: "Button",
  props: ["txt", "type"],
};
</script>

<style scoped>
.btn {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}

.btn:hover {
  background-color: transparent;
  color: #222;
}
</style>