<script lang="ts">
  import { onDestroy } from 'svelte';
  import { key } from './key';
  import { getContext } from 'svelte';

  interface FrequencyChange {
    value: number;
    endTime: number;
  }

  export let dest: AudioNode | undefined = undefined;
  export let type: OscillatorType = 'sine';
  export let frequency: number | FrequencyChange = 440;

  const ctx = getContext<AudioContext>(key);

  const node = ctx.createOscillator();
  node.type = type;
  setFrequency(frequency);

  function setFrequency(freq: number | FrequencyChange) {
    if (typeof freq === 'number') {
      node.frequency.setValueAtTime(freq, ctx.currentTime);
    } else {
      node.frequency.linearRampToValueAtTime(freq.value, ctx.currentTime + freq.endTime);
    }
  }

  onDestroy(() => {
    node.stop();
  });

  if (dest) {
    node.connect(dest);
  }

  node.start();

  $: {
    setFrequency(frequency);
  }

  $: {
    node.type = type;
  }
</script>

<slot {node} />