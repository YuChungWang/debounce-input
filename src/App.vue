<template>
  <section class="normal">
    <div class="container">
      <label for="normal">Normal Input:</label>
      <input id="normal" type="text" name="normal" @input="changeHandler(InputType.Normal, $event)" />
    </div>
    <p class="result">{{ normalInput }}</p>
  </section>
  
  <section class="debounce">
    <div class="container">
      <label for="debounce">Debounce Input:</label>
      <input id="debounce" type="text" name="debounce" @input="debounceChangeHandler(InputType.Debounce, $event)" />
    </div>
    <p class="result">{{ debounceInput }}</p>
  </section>
</template>

<script setup lang="ts">
import { ref } from 'vue';

enum InputType {
  Normal = 'normal',
  Debounce = 'debounce',
}

const normalInput = ref('');
const debounceInput = ref('');

const debounce = (fn: (...args: any) => void, delay: number) => {
  let timer: NodeJS.Timer | null = null;

  return (...args: any) => {
    if (timer) {
      clearTimeout(timer);
    }
    timer = setTimeout(() => {
      fn.apply(this, args);
    }, delay);
  }
};
const changeHandler = (inputType: InputType, event: Event) => {
  const { value } = event.target as HTMLInputElement;
  switch (inputType) {
    case InputType.Normal:
      normalInput.value = value;
      break;
    case InputType.Debounce:
      debounceInput.value = value;
      break;
    default:
      break;
  }
};
const debounceChangeHandler = debounce(changeHandler, 1000);
</script>

<style lang="scss" scoped>
section {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
  width: 360px;
  height: 60px;
  margin-bottom: 20px;

  .container {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    width: 100%;
    height: 50%;

    label,
    input {
      flex: 1;
      text-align: left;
    }
  }

  .result {
    width: 100%;
    height: 50%;
    margin: 0;
    text-align: left;
    overflow: hidden;
    text-overflow: ellipsis;
  }
}
</style>
