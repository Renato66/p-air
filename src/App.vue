<script setup lang="ts">
import { ref, onMounted } from 'vue';
import TodoItem from './components/TodoItem.vue';
import TodoAdd from './components/TodoAdd.vue';

interface Todo {
  id: string;
  text: string;
  completed: boolean;
}

const isLoading = ref(false);
const items = ref<Todo[]>([]);

async function populateItems() {
  // Simulate "async" fetching of data
  items.value = [
    { id: '1', text: 'Morning walk', completed: true },
    { id: '2', text: 'Lunch', completed: false },
    { id: '3', text: 'Dinner with IPA', completed: false },
    { id: '4', text: 'Night Reading', completed: false }
  ];
}

function removeItem(item: Todo) {
  items.value = items.value.filter((i) => i.id !== item.id);
}

function toggleItem(item: Todo) {
  items.value = items.value.map((i) => {
    if (i.id === item.id) {
      return { ...i, completed: !i.completed };
    }
    return i;
  });
}

function addItem(text: string) {
  const newItem = {
    id: Date.now().toString(),
    text,
    completed: false
  };
  items.value.push(newItem);
}

onMounted(async () => {
  try {
    isLoading.value = true;
    await populateItems();
  } catch (error) {
    console.error(error);
  } finally {
    isLoading.value = false;
  }
});
</script>

<template>
  <main>
    <section class="page">
      <h1 class="page__header">ToDo</h1>
      <TodoAdd @add="addItem" />
      <transition-group name="collapse" tag="ul" class="page__todo-list" mode="out-in">
        <TodoItem
          v-for="item in items"
          :key="item.id"
          :item="item"
          @remove="removeItem"
          @toggle="toggleItem"
        />
        <li v-if="items.length === 0" key="all-done" class="all-done">All done!</li>
      </transition-group>
    </section>
  </main>
</template>

<style lang="scss">
.page {
  background: #ffffff;
  max-width: 28.375rem;
  width: 100%;
  box-shadow: 0px 4px 4px 0px #00000040;
  border-radius: 1.875rem;
  padding: 2.5rem 3.75rem 1.4rem;
  box-sizing: border-box;

  &__header {
    font-weight: 700;
    font-size: 72px;
    color: #5e5e5e;
    margin: 0.675rem 0;
    letter-spacing: 6px;
  }
  &__todo-add {
    padding-top: 1.1rem;
    box-sizing: border-box;
    min-height: 4.5rem;
    margin-bottom: 1rem;
  }
  &__todo-list {
    list-style: none;
    margin: 0;
    padding: 0;
    max-height: 17rem;
    overflow: auto;
    position: relative;

    &::after {
      content: '';
      position: sticky;
      display: block;
      bottom: 0;
      left: 0;
      width: 100%;
      height: 2rem;
      background: linear-gradient(to bottom, rgba(255, 255, 255, 0), rgba(255, 255, 255, 1));
      pointer-events: none;
    }
  }
}

.all-done {
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.5rem;
  color: #5e5e5e;
}

</style>