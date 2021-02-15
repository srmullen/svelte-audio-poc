<script lang="ts">
  import { key } from './key';
  import { setContext } from 'svelte';

  const ctx = new AudioContext();

  setContext(key, ctx);

  export function resume() {
    ctx.resume();
  }

  export function suspend() {
    ctx.suspend();
  }
</script>

<slot 
  node={ctx.destination} 
/>