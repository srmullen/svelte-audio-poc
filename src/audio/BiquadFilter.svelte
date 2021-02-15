<script lang="ts">
  import { key } from './key';
  import { getContext } from 'svelte';

  export let dest: AudioDestinationNode;
  export let type: BiquadFilterType = 'lowpass';
  export let frequency = 440;

  const ctx = getContext<AudioContext>(key);

  const node = ctx.createBiquadFilter();

  $: node.type = type;
  $: node.frequency.value = frequency;

  node.connect(dest);
</script>

<slot {node} />