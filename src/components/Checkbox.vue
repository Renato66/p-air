<script setup lang="ts">
import CheckboxChecked from '../assets/checkbox-checked.svg';
import CheckboxUnchecked from '../assets/checkbox-unchecked.svg';

interface Props {
  checked: boolean;
}

defineProps<Props>();

</script>

<template>
  <span class="checkbox">
    <img
      :src="checked ? CheckboxChecked : CheckboxUnchecked"
      alt="Checkbox"
      aria-hidden="true"
    />
  </span>
</template>

<style lang="scss">
.checkbox {
  display: inline-flex;
  align-items: center;
  justify-content: center;
}
</style>