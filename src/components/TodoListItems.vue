<template>
  <li>
    <span class="item" :class="todoItemClass" @click="toggleItem">{{ todoItem.title }}</span> <!-- computed 안 쓰면 어떻게 쓰지? this.todoItem.done ? "completed" : null + computed를 해도 모르네 -->
    <button @click="removeItem">삭제</button>
  </li>
</template>

<script lang="ts">
import Vue, {PropType} from 'vue'
import {Todo} from "@/App.vue";

    export default Vue.extend ({
      props: { // 여길 아예 놓쳤음. { } 활용 / Object / type
        todoItem: {
          type: Object as PropType<Todo>,
        },
        index: {
          type: Number
        }
      },
      computed: {
        todoItemClass(): string | any {
          return this.todoItem.done ? "completed" : null;
        }
      },
      methods: {
        toggleItem() {
          this.$emit("toggle", this.todoItem, this.index)
        },
        removeItem() {
          this.$emit("remove", this.index)
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