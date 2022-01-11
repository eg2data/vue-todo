<template>
  <div>
    <label for="todo-input"></label>
    <input id="todo-input" type="text" :value="item" @input="handleInput"/>
    <button type="button" @click="addItem">추가</button>
  </div>
</template>

<script lang="ts">
    import Vue from 'vue'

    export default Vue.extend ({
      props: ["item"],
      methods: {
          handleInput(event: InputEvent) {
            const eventTarget = event.target as HTMLInputElement
            this.$emit("input", eventTarget.value)
          },
          addItem() {
            this.$emit("add")
          }
      }
    })
</script>

<style scoped>

</style>