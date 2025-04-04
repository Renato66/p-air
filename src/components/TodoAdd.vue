<script setup lang="ts">
import { ref, watch } from 'vue';
import AddIcon from '../assets/add.svg';

const emit = defineEmits<{
  add: [text: string];
}>();

const isFormOpen = ref(false);
const newItemText = ref('');
const input = ref<HTMLInputElement | null>(null);

watch(isFormOpen, (value) => {
  if (value) {
    setTimeout(() => {
      input.value?.focus();
    }, 500);
  } else {
    newItemText.value = '';
  }
});

function addItem() {
  if (!newItemText.value) return;
  emit('add', newItemText.value);
  isFormOpen.value = false;
}
</script>

<template>
  <section class="page__todo-add">
    <transition name="fade" mode="out-in">
      <button v-if="!isFormOpen" class="todo-add__button" @click="isFormOpen = true">
        <img :src="AddIcon" alt="Add task" />
        Add New Task
      </button>
      <div v-else key="form" class="todo-add__form">
        <input
          v-model.trim="newItemText"
          autofocus
          ref="input"
          @keyup.enter="addItem"
          @keyup.esc="isFormOpen = false"
          placeholder="New task..."
          class="todo-add__input"
        />
        <button class="todo-add__button" @click="addItem">
          <img :src="AddIcon" alt="Add task" />
        </button>
      </div>
    </transition>
  </section>
</template>

<style lang="scss">
.todo-add {
  &__form {
    display: flex;
    gap: 10px;
    align-items: center;
  }

  &__input {
    flex: 1;
    padding: 0.5rem 1rem;
    font-size: 16px;
    color: #000000;
    border: 1px solid #d9d9d9;
    border-radius: 1.875rem;
  }

  &__button {
    color: #b47aea;
    display: flex;
    align-items: center;
    gap: 0.8rem;
    background: none;
    border: none;
    cursor: pointer;
    padding: 0 0.25rem;
    font-weight: 700;
    font-size: 24px;
  }
}
</style>