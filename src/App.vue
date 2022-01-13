<template>
  <div>
    <header>
      <h1> 13th January, 2022 vue-ts blank test. </h1>
    </header>
    <main>
      <TodoList :item="todoItem" @update="updateTodoItem" @add="addTodoItem"></TodoList>
      <ul>
        <TodoItemList v-for="(todoItem, index) in todoItems" :key="index" :item="todoItem" :index="index" @toggle="toggleTodoItem" @remove="removeTodoItem"></TodoItemList> <!-- (todoItem, ) 과 :item="todoItem"이 동일해야 하네 -->
      </ul>
    </main>
  </div>
</template>

<script lang="ts">
    import Vue from 'vue'
    import TodoList from "@/components/TodoList.vue";
    import TodoItemList from "@/components/TodoItemList.vue";

    const STORAGE_KEY = "13-jan-vue-ts"
    const storage = {
      save(value: Todo[]) {
        const parsed = JSON.stringify(value)
        localStorage.setItem(STORAGE_KEY, parsed)
      },
      fetch(): Todo[] {
        const value = localStorage.getItem(STORAGE_KEY) || "[]"
        const result = JSON.parse(value)
        return result
      },
    }

    export interface Todo {
      title: string,
      done: boolean
    }

    export default Vue.extend ({
      components: {TodoItemList, TodoList},
      data() {
        return {
          todoItem: "init",
          todoItems: [] as Todo[]
        }
      },
      methods: {
        updateTodoItem(value: string) {
          this.todoItem = value

        },
        initTodoItem() {
          this.todoItem = ""
        },
        addTodoItem() {
          const value = this.todoItem
          const values: Todo = {
            title: value,
            done: false
          }
          this.todoItems.push(values)
          storage.save(this.todoItems)
          this.initTodoItem()
        },
        fetchTodoItem() {
          this.todoItems = storage.fetch().sort((a, b) => {
            if (a.title > b.title) {
              return 1;
            }
            if (a.title < b.title) {
              return -1;
            }
            return 0;
          });
          storage.save(this.todoItems)
        },
        toggleTodoItem(item: Todo, index: number) {
          this.todoItems.splice(index, 1, {
            ...item,
            done: !item.done
          })
          storage.save(this.todoItems)
        },
        removeTodoItem(index: number) {
          this.todoItems.splice(index, 1)
          storage.save(this.todoItems)
        }
      },
      created() {
        this.fetchTodoItem();
      }
    })
</script>

<style scoped>

</style>