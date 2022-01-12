<template>
  <div>
    <header>
      <h1> 12th January, 2022 vue-ts blank test. </h1>
    </header>
    <main>
      <TodoList :item="todoItem" @input="updateTodoItem" @add="addTodoItem"></TodoList>
      <ul>
        <TodoListItems v-for="(todoItem, index) in todoItems" :key="index" :todoItem="todoItem" :index="index"
        @remove="removeTodoItem" @toggle="toggleTodoItem"></TodoListItems>
      </ul>
    </main>

  </div>
</template>

<script lang="ts">
    import Vue from 'vue'
    import TodoList from "@/components/TodoList.vue";
    import TodoListItems from "@/components/TodoListItems.vue";

    export interface Todo {
      title: string,
      done: boolean
    }

    const STORAGE_KEY = "12-jan-vue-ts"
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
      components: {TodoListItems, TodoList},
      data() {
        return {
          todoItem: "init",
          todoItems: [] as Todo[]
        }
      },
      methods: {
        updateTodoItem(value: string) {
          this.todoItem = value;
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
        fetchTodoItem() {
          this.todoItems = storage.fetch().sort((a, b) => { // 새로고침 안 누르고 바로 정렬되게 하려면 어떻게해야할까? 고민해보고 적용해보기
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
        toggleTodoItem(value: Todo, index: number) {
          this.todoItems.splice(index, 1, {
            ...value,
            done: !value.done
          })
          storage.save(this.todoItems)
        },
        removeTodoItem(index: number) {
          this.todoItems.splice(index, 1)
          storage.save(this.todoItems)
        }

      },
      created() {  // 이게 정확히 무엇을 의미하는지 좀 알아야겠다. 뭔 의미가 있지? 없는 것과는 뭔 차이가 있지?
        this.fetchTodoItem()
      }

    })
</script>

<style scoped>

</style>