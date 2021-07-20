<template>
  <button class="btn-submit" type="submit" @click="$emit('onClick')">{{ text }}</button>
</template>

<script>

export default {
  name: 'Button',
  props: ['text']
}
</script>

<style scoped>
.btn-submit {
  width: 100%;
  height: 40px;
  border-radius: 8px;
  outline: none;
  border: none;
  background-color: #4395d8;
  color: #fff;
  font-weight: bold;
  font-size: 14px;
  transition: .4s;
}

.btn-submit:hover {
  background-color: #3672a3;
}
</style>
