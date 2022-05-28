<template>
  <div class="alert-container">
    <div v-if="showAlert" :class="alert.type" class="alert-box">
      <p>{{ alert.messege }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    showAlert: {
      type: Boolean,
    },
    alert: {
      type: Object,
    },
  },
};
</script>

<style lang="scss" scoped></style>
