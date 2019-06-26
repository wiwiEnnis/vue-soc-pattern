<template>
  <div>
    <h1>Component Pattern</h1>
    <p>current case: {{ textTransformer.case }}</p>
    <p>times changing case: {{ timesChangingCase }}</p>
    <p>transformed text: {{ transformedText }}</p>
    <input type="text" v-model="text" />
    <div>
      <button @click="textTransformer.toLowerCase">toLowerCase</button>
      <button @click="textTransformer.toUpperCase">toUpperCase</button>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Watch } from 'vue-property-decorator';

const createTextTransformer = (props: { text: string }) => {
  @Component
  class Counter extends Vue {
    public case: string = 'lower';

    get props() {
      return props;
    }
    get transformedText() {
      return this.case === 'lower'
        ? props.text.toLowerCase()
        : props.text.toUpperCase();
    }

    public toLowerCase() {
      this.case = 'lower';
    }
    public toUpperCase() {
      this.case = 'upper';
    }
  }

  return new Counter();
};

@Component
export default class ComponentPattern extends Vue {
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
    return this.textTransformer.transformedText;
  }

  @Watch('textTransformer.case')
  public watchCase() {
    this.timesChangingCase += 1;
  }

  beforeDestroy() {
    this.textTransformer.$destroy();
  }

  public toLowerCase() {
    this.transformedText.toLowerCase();
  }
  public toUpperCase() {
    this.textTransformer.toUpperCase();
  }
}
</script>
