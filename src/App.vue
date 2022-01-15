<template>
  <div>
    <header>
      <h1> 15th January, 2022 vue-ts blank test. </h1>
    </header>
    <main>
      <TodoLIst :item="todoItem" @update="updateTodoItem" @add="addTodoItem"></TodoLIst>
      <ul>
        <TodoListItems v-for="(todoItem, index) in todoItems" :key="index" :item="todoItem" :index="index" @remove="removeTodoItem" @toggle="toggleTodoItem"></TodoListItems>
      </ul>
    </main>
  </div>
</template>

<script lang="ts">
    import Vue from 'vue'
    import TodoLIst from "@/components/TodoLIst.vue";
    import TodoListItems from "@/components/TodoListItems.vue";

    const STORAGE_KEY = "15-jan-vue-ts"
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

    export interface Todo {
      title : string,
      done : boolean
    }

    export default Vue.extend ({
      components: {TodoListItems, TodoLIst},
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
        initTodoItem() {
          this.todoItem = ""
        },
        fetchTodoItem() { // type 관련해서 좀 헤매고 있네. Todo와 Todo[]
          this.todoItems = storage.fetch().sort((a: Todo, b: Todo) => {
            if (a.title > b.title) {
              return 1;
            }
            if (a.title < b.title) {
              return -1;
            }
            return 0;
          })
          storage.save(this.todoItems) // 여기서 왜..? 왜 error가..?
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