<template>
  <div>
    <label for="todo-list"> Today's tasks </label>
    <input id="todo-list" type="text" @input="updateItem" :value="item"/>
    <button type="button" @click="addItem"> ADD </button>
  </div>
</template>

<script lang="ts">
    import Vue from 'vue'

    export default Vue.extend ({
      props: {
        item: {
          type: String,
          required: true
        }
      },
      methods: {
        updateItem(event: InputEvent) {
          const eventTarget = event.target as HTMLInputElement
          this.$emit("update", eventTarget.value)
        },
        addItem() {
          this.$emit("add")
        }
      }
    })
</script>

<style scoped>

</style>