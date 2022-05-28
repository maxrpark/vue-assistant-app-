<template>
  <form @submit.prevent="handleSubmit" class="form-container">
    <input
      v-model="task"
      type="text"
      class="input-control"
      placeholder="Type something..."
      maxlength="25"
    />
  </form>
</template>

<script>
export default {
  data() {
    return {
      task: '',
    };
  },
  methods: {
    handleSubmit() {
      this.$emit('handleSubmit', this.task);
    },
  },
};
</script>

<style lang="scss" scoped></style>
