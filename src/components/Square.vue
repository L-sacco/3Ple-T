<script setup>
const prop = defineProps(["innerText"]);
const emit = defineEmits(["click"]);

const onClick = () => {
  emit("click");
};
</script>

<template>
  <div>
    <a
      @click="onClick()"
      class="square display-1"
      :class="{ 'text-success': innerText == 'X', 'text-danger': innerText == 'O' }"
      >
      {{ innerText }}
    </a>
  </div>
</template>

<style scoped>
.square {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 150px;
  height: 150px;
  text-decoration: none;
}
</style>
