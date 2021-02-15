<script lang="ts">
  import { key } from './key';
  import { getContext } from 'svelte';

  export let dest: AudioDestinationNode;

  const ctx = getContext<AudioContext>(key);

  const node = ctx.createPanner();

  node.connect(dest);
</script>

<slot {node} />