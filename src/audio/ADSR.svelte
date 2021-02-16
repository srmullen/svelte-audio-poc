<script lang="ts">
  // Attack Decay Sustain Release
  import adsr from 'a-d-s-r';
  import Gain from './Gain.svelte';

  export let dest: AudioNode | AudioDestinationNode | AudioParam;

  export let attack = 0;
  export let decay = 0;
  export let sustain = 0;
  export let release = 0;

  let node: GainNode;
</script>

<Gain let:node {dest}>
  <slot {node} />
</Gain>