<template>
  <div>
    <li>
      <span @click="toggleItem" class="item" :class="completeItem"> {{ item.title }} </span>
      <button type="button" @click="removeItem">Delete</button>
    </li>
  </div>
</template>

<script lang="ts">
import Vue, {PropType} from 'vue'
import {Todo} from "@/App.vue";

    export default Vue.extend ({
      props: {
        item: {
          type: Object as PropType<Todo>,
        },
        index: {
          type: Number,
        }
      },
      methods: {
        toggleItem() {
          this.$emit("toggle", this.item, this.index)
        },
        removeItem() {
          this.$emit("remove", this.index)
        }
      },
      computed: {
        completeItem(): string | null {
          return this.item.done ? "completed" : null
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