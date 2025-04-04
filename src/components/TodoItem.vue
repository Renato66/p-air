<script setup lang="ts">
import DeleteIcon from '../assets/delete.svg';
import Checkbox from './Checkbox.vue';

interface TodoItemProps {
  id: string;
  text: string;
  completed: boolean;
}

defineProps<{
  item: TodoItemProps;
}>();

const emit = defineEmits<{
  remove: [item: TodoItemProps];
  toggle: [item: TodoItemProps];
}>();
</script>

<template>
  <li class="todo-item">
    <button class="todo-item__delete" @click="emit('remove', item)">
      <img :src="DeleteIcon" alt="Delete task" />
    </button>
    <label
      class="todo-item__label"
      :for="`checkbox-${item.id}`"
      role="checkbox"
      :aria-checked="item.completed"
      tabindex="0"
      @click="emit('toggle', item)"
      @keydown.space.prevent="emit('toggle', item)"
    >
      <Checkbox :id="item.id" :checked="item.completed" @toggle="emit('toggle', item)" />
      <span
        class="todo-item__text"
        :title="item.text"
        :class="{ 'todo-item__text--completed': item.completed }"
      >
        {{ item.text }}
      </span>
    </label>
  </li>
</template>

<style lang="scss">
.todo-item {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 0.4rem 0;

  &__delete {
    background: none;
    border: none;
    cursor: pointer;
    margin-top: 4px;
    padding: 0;
  }

  &__text {
    font-weight: 500;
    font-size: 24px;
    color: #000000;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;

    &--completed {
      text-decoration: line-through;
    }
  }

  &__label {
    display: flex;
    align-items: center;
    gap: 1.5rem;
    min-width: 0;
    flex: 1;
    cursor: pointer;
  }
}
</style>