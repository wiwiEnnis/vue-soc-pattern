<template>
  <div>
    <TextTransformer ref="textTransformer" :text="text" />

    <template v-if="businessLogicMounted">
      <h1>Renderless Pattern</h1>
      <p>current case: {{ TextTransformerModel.textCase }}</p>
      <p>times changing case: {{ TextTransformerModel.timesChangingCase }}</p>
      <p>transformed text: {{ TextTransformerModel.transformedText }}</p>
      <input type="text" v-model="text" />
      <div>
        <button @click="TextTransformerModel.toLowerCase">toLowerCase</button>
        <button @click="TextTransformerModel.toUpperCase">toUpperCase</button>
      </div>
    </template>
  </div>
</template>

<script lang="ts">
import { CreateElement } from 'vue';
import { Component, Vue, Prop, Watch } from 'vue-property-decorator';

@Component
class TextTransformer extends Vue {
  @Prop({ type: String, default: '' })
  public text!: string;

  public textCase: string = 'lower';
  public timesChangingCase: number = 0;

  get transformedText() {
    return this.textCase === 'lower'
      ? this.text.toLowerCase()
      : this.text.toUpperCase();
  }

  @Watch('textCase')
  public watchTextCase() {
    this.timesChangingCase += 1;
  }

  public toLowerCase() {
    this.textCase = 'lower';
  }
  public toUpperCase() {
    this.textCase = 'upper';
  }

  render() {
    return null;
  }
}

@Component({
  components: {
    TextTransformer
  }
})
export default class RenderlessPattern extends Vue {
  public $refs!: {
    textTransformer: TextTransformer;
  };

  public text: string = 'Hello World!';
  public businessLogicMounted: boolean = false;

  get TextTransformerModel() {
    return this.$refs.textTransformer;
  }

  public mounted() {
    this.businessLogicMounted = true;
  }
}
</script>
