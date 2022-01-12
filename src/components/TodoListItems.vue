<template>
  <div>
    <li>
      <span class="item" :class="completeItem" @click="toggleItem"> {{ todoItem.title }} </span>
      <button type="button" @click="removeItem">삭제</button>
    </li>
  </div>
</template>

<script lang="ts">
import Vue, {PropType} from 'vue'
import {Todo} from "@/App.vue";

    export default Vue.extend ({
      props: {
        todoItem: {
          type: Object as PropType<Todo>,
        },
        index: {
          type: Number,
        }
      },
      methods: {
        toggleItem() {
          this.$emit("toggle", this.todoItem, this.index)
        },
        removeItem() {
          this.$emit("remove", this.index)
        }
      },
      computed: {
        completeItem(): string | null {
          return this.todoItem.done ? "completed" : null
        }
      }
    })
</script>

<style scoped>
.item {
  cursor: pointer;
}
.completed {
  text-decoration: line-through;
}

</style>