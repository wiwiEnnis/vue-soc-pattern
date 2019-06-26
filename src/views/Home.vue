<template>
  <div class="home">
    <FunctionApiPattern />
    <ScopedSlotPattern />
    <RenderlessPattern />
    <ComponentPattern />
    <FunctionPattern />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import FunctionApiPattern from '@/components/FunctionApiPattern.vue';
import ScopedSlotPattern from '@/components/ScopedSlotPattern.vue';
import ComponentPattern from '@/components/ComponentPattern.vue';
import FunctionPattern from '@/components/FunctionPattern.vue';
import RenderlessPattern from '@/components/RenderlessPattern.vue';

@Component({
  components: {
    FunctionApiPattern,
    ScopedSlotPattern,
    ComponentPattern,
    FunctionPattern,
    RenderlessPattern
  }
})
export default class Home extends Vue {}
</script>
