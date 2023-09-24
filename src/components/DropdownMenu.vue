<template>
  <label>{{ text }}</label>
        <select v-model="selectedOption" @change="emitSelectedOption">
          <option value=""></option>
          <option value="ascending">Low - High</option>
          <option value="descending">High - Low</option>
          <option value="relevance">Relevance</option>
        </select>
</template>

<script>
export default {
  name: 'DropdownMenu',
  props: ['text'],
  emits: ['selectedOption'],
  data() {
    return {
      selectedOption: '',
    };
  },
  methods: {
    emitSelectedOption() {
      this.$emit('selectedOption', this.selectedOption);
    },
  },
}

</script>