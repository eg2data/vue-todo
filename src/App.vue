<template>
  <div>
    <header>
      <h1> 14th January, 2022 vue-ts blank test. </h1>
    </header>
    <main>
      <TodoList :item="todoItem" @update="updateTodoItem" @add="addTodoItem"></TodoList>
      <ul>
       <TodoItemList v-for="(todoItem, index) in todoItems" :key="index" :item="todoItem" :index="index"
       @remove="removeTodoItem" @toggle="toggleTodoItem"></TodoItemList>
      </ul>
    </main>
  </div>
</template>

<script lang="ts">
    import Vue from 'vue'
    import TodoList from "@/components/TodoList.vue";
    import TodoItemList from "@/components/TodoItemList.vue";

    export interface Todo {
      title: string,
      done: boolean
    }

    const STORAGE_KEY = "14-jan-vue-ts"
    const storage = {
      save(value: Todo[]) {
        const parsed = JSON.stringify(value)
        localStorage.setItem(STORAGE_KEY, parsed)
      },
      fetch(): Todo[] {
        const value = localStorage.getItem(STORAGE_KEY) || "[]"
        const result = JSON.parse(value)
        return result
      }
    }

    export default Vue.extend ({
      components: {TodoItemList, TodoList},
      data() {
        return {
          todoItem: "",
          todoItems: [] as Todo[]
        }
      },
      methods: {
        updateTodoItem(value: string) {
          this.todoItem = value
        },
        addTodoItem() {
          const value = this.todoItem
          const values: Todo = {
            title: value,
            done: false
          }
          this.todoItems.push(values)
          storage.save(this.todoItems)
          this.initTodoItem() // 이게 지금 왜 안먹지? => 찾았다. TodoList > input 에서 :value를 안줬어. 그러니 안먹는 것처럼 보인거지. ok.
        },
        initTodoItem() {
          this.todoItem = ""
        },
        fetchTodoItem() {
          this.todoItems = storage.fetch().sort((a: any, b: any) => {
            if (a.title > b.title) {
              return -1
            }
            if (a.title < b.title) {
              return 1
            }
            return 0
          })
          storage.save(this.todoItems)
        },
        removeTodoItem(index: number) {
          this.todoItems.splice(index, 1)
          storage.save(this.todoItems)
        },
        toggleTodoItem(item: Todo, index: number) {
          this.todoItems.splice(index, 1, {
            ...item,
            done: !item.done
          })
          storage.save(this.todoItems)
        }
      },
      created() {
        this.fetchTodoItem()
      }
    })
</script>

<style scoped>

</style>