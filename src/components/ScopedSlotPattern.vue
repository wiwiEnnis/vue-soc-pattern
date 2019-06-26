<template>
  <TextTransformer
    v-slot:default="{
      textCase,
      timesChangingCase,
      transformedText,
      toUpperCase,
      toLowerCase
    }"
    :text="text"
  >
    <div>
      <h1>Scoped Slot Pattern</h1>
      <p>current case: {{ textCase }}</p>
      <p>times changing case: {{ timesChangingCase }}</p>
      <p>transformed text: {{ transformedText }}</p>
      <input type="text" v-model="text" />
      <div>
        <button @click="toLowerCase">toLowerCase</button>
        <button @click="toUpperCase">toUpperCase</button>
      </div>
    </div>
  </TextTransformer>
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
    return this.$scopedSlots.default
      ? this.$scopedSlots.default({
          textCase: this.textCase,
          timesChangingCase: this.timesChangingCase,
          transformedText: this.transformedText,
          toLowerCase: this.toLowerCase,
          toUpperCase: this.toUpperCase
        })
      : null;
  }
}

@Component({
  components: {
    TextTransformer
  }
})
export default class ScopedSlotPattern extends Vue {
  public text: string = 'Hello World!';
  public businessLogicMounted: boolean = false;

  public mounted() {
    this.businessLogicMounted = true;
  }
}
</script>
