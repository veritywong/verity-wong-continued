<template>
  <label for="checkbox">{{ text }}</label>
    <input
      @click="toggle"
      type="checkbox"
      id="checkbox"
    />
</template>


<script>
export default {
  name: 'Checkbox',
  props: ['text'],
  emits: ['click'],
  data() {
    return {
      status: false,
    };
  },
  methods: {
    toggle() {
      this.status = !this.status;
      this.$emit('click')
    },
  }
}
</script>