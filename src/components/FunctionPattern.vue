<template>
  <div>
    <h1>Function Pattern</h1>
    <p>current case: {{ textTransformer.state.case }}</p>
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
import { Component, Vue, Watch } from 'vue-property-decorator';

const createTextTransformer = (props: { text: string }) => {
  const state = Vue.observable({
    case: 'lower'
  });

  const getters = {
    get props(): typeof props {
      return {
        text: props.text
      };
    },
    get transformedText() {
      return state.case === 'lower'
        ? props.text.toLowerCase()
        : props.text.toUpperCase();
    }
  };

  const toLowerCase = () => (state.case = 'lower');
  const toUpperCase = () => (state.case = 'upper');

  return {
    state,
    getters,
    methods: {
      toLowerCase,
      toUpperCase
    }
  };
};

@Component
export default class FunctionPattern extends Vue {
  public text: string = 'Hello World!';
  public timesChangingCase: number = 0;
  public textTransformer!: ReturnType<typeof createTextTransformer>;

  data() {
    const _this = this;
    const props = {
      get text() {
        return _this.text;
      }
    };

    return {
      textTransformer: createTextTransformer(props)
    };
  }

  get transformedText() {
    return this.textTransformer.getters.transformedText;
  }

  @Watch('textTransformer.case')
  public watchCase() {
    this.timesChangingCase += 1;
  }

  public toLowerCase() {
    this.textTransformer.methods.toLowerCase();
  }
  public toUpperCase() {
    this.textTransformer.methods.toUpperCase();
  }
}
</script>
