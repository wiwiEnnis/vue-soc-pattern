<template>
  <div>
    <h1>Function Api Pattern</h1>
    <p>current case: {{ textCase }}</p>
    <p>times changing case: {{ timesChangingCase }}</p>
    <p>transformed text: {{ transformedText }}</p>
    <input type="text" v-model="text" />
    <div>
      <button @click="toLowerCase">toLowerCase</button>
      <button @click="toUpperCase">toUpperCase</button>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import {
  value,
  computed,
  watch,
  createComponent,
  Wrapper
} from 'vue-function-api';

const useTextTransformer = (props: { text: Wrapper<string> }) => {
  // Props
  const { text } = props;

  // State
  const textCase = value('lower');

  // Computed
  const transformedText = computed(() =>
    textCase.value === 'lower'
      ? text.value.toLowerCase()
      : text.value.toUpperCase()
  );

  // Methods
  const toLowerCase = () => {
    textCase.value = 'lower';
  };
  const toUpperCase = () => {
    textCase.value = 'upper';
  };

  return {
    textCase,
    transformedText,
    toLowerCase,
    toUpperCase
  };
};

const FunctionApiPattern = createComponent({
  setup() {
    const text = value('Hello World!');
    const timesChangingCase = value(0);
    const textTransformer = useTextTransformer({ text });
    const {
      textCase,
      transformedText,
      toLowerCase,
      toUpperCase
    } = textTransformer;

    watch(textCase, () => (timesChangingCase.value += 1));

    return {
      text,
      textCase,
      transformedText,
      timesChangingCase,
      toLowerCase,
      toUpperCase
    };
  }
});

export default FunctionApiPattern;
</script>
