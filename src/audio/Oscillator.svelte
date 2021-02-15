<script lang="ts">
  import { key } from './key';
  import { getContext } from 'svelte';

  export let dest: AudioNode | undefined = undefined;
  export let type: OscillatorType = 'sine';
  export let frequency = 440;

  const ctx = getContext<AudioContext>(key);

  const node = ctx.createOscillator();
  node.type = type;
  node.frequency.setValueAtTime(frequency, ctx.currentTime);

  if (dest) {
    node.connect(dest);
  }

  node.start();

  $: {
    node.frequency.setValueAtTime(frequency, ctx.currentTime);
  }

  $: {
    node.type = type;
  }
</script>

<slot {node} />