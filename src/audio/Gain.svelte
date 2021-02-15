<script lang="ts">
  import { key } from './key';
  import { getContext } from 'svelte';

  export let dest: AudioNode | AudioDestinationNode | AudioParam;
  export let gain: number = 1;

  const ctx = getContext<AudioContext>(key);

  const node = ctx.createGain();
  node.gain.setValueAtTime(gain, ctx.currentTime);

  if (dest) {
    // @ts-ignore
    node.connect(dest);
  }

  $: {
    node.gain.setValueAtTime(gain, ctx.currentTime);
  }
  
</script>

<slot {node} />