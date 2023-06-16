<template>
  <Layout>
    <form action="form-tasks" @submit.prevent="createTask">
      <input
        type="text"
        placeholder="Add a new task"
        class="form-tasks__input"
        v-model="taskName"
      />
      <button class="form-tasks__button">Make Task</button>
    </form>

    <div class="no-result" v-if="!todoStore.task.length && todoStore.loading">
      <span>😧</span>
      <br />
      <span class="no-result__text">No task yet...</span>
    </div>

    <template v-else-if="todoStore.task.length && !todoStore.loading">
      <div class="tasks-container">
        <div class="card-task" v-for="task in todoStore.task">
          <input
            type="checkbox"
            class="card-task__checkbox"
            :checked="task.done"
            @click.prevent="updateTask(task.id)"
          />

          <span class="card-task__name">{{ task.name }}</span>

          <button
            type="button"
            class="card-task__button"
            @click.prevent="deleteTask(task.id)"
          >
            ❌
          </button>
        </div>
      </div>
    </template>

    <div class="loading-container">
      <p>Loading...</p>
    </div>
  </Layout>
</template>

<script lang="ts">
import Layout from './layouts/default.vue';
import { defineComponent, ref } from 'vue';
import { useTodoStore } from './store/todo';

export default defineComponent({
  components: { Layout },
  setup() {
    const taskName = ref('');
    const todoStore = useTodoStore();

    function createTask(): void {
      todoStore.addTask(taskName.value);
      taskName.value = '';
    }

    function deleteTask(id: string): void {
      todoStore.deleteTask(id);
    }

    function updateTask(id: string): void {
      todoStore.updateTask(id);
    }

    return {
      taskName,
      createTask,
      deleteTask,
      updateTask,

      todoStore,
    };
  },
});
</script>

<style lang="scss">
// Import font
@import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@300;400;500;600;700&display=swap');

// Declaration of variables
$primary-color: #db7d11;
$white-color: #0000;
$gray-color: #0002;
$border: 10px;
$padding: 10px;

body {
  font-family: 'Quicksand', sans-serif;
  background-color: #333;
  color: white;

  .form-tasks {
    width: 100%;
    display: flex;

    &__input {
      all: initial;
      font-family: 'Quicksand', sans-serif;
      background-color: $gray-color;
      width: calc(65% - 3.5rem);
      padding: $padding;
      border-radius: $border;
    }

    &__button {
      all: initial;
      font-family: 'Quicksand', sans-serif;
      width: 35%;
      background-color: $primary-color;
      padding: $padding;
      border-radius: $border;
      color: white;
      font-weight: bold;
      text-align: center;
      text-transform: uppercase;
      margin-left: 1rem;
    }
  }

  .no-result,
  .loading-container {
    text-align: center;
  }

  .tasks-container {
    width: 100%;

    .card-task {
      background: $gray-color;
      border-radius: $border;
      padding: $padding;
      margin-bottom: 1rem;

      &__checkbox {
        transition: all 0.4 ease;
      }

      &__name {
        text-transform: uppercase;
        margin-left: 1rem;
      }

      &.done {
        .card-tasks__name {
          text-decoration: line-trough;
          opacity: 0.7;
        }
      }

      &__button {
        all: initial;
        float: right;
        cursor: pointer;
      }
    }
  }
}
</style>
