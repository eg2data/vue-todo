<template>
  <div>
    <label for="todo-list">오늘 할 일: </label>
    <input id="todo-list" type="text" :value="item" @input="updateItem"/>
    <button type="text" @click="addItem">추가</button>
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