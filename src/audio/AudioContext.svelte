<script lang="ts">
  import { createEventDispatcher } from 'svelte';
  import { key } from './key';
  import { setContext } from 'svelte';

  const ctx = new AudioContext();

  const dispatch = createEventDispatcher();

  ctx.onstatechange = (e) => {
    dispatch('statechange', ctx.state);
  }

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