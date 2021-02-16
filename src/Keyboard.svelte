<script lang="ts">
  import { Context, Gain, Oscillator } from './audio';
  import { noteToHz } from './pitch';

  const keys: string[] = ['a4', 'b4', 'c5', 'd5', 'e5', 'f5', 'g5', 'a5'];

  let triggers: { [key: string]: boolean }
  $: triggers = {};

  let key: string | undefined;

  let ctx: Context;

  function playNote(note: string) {
    // console.log(note, noteToHz(note));
    // ctx.resume();
    // triggers[note] = true;
    // setTimeout(() => {
    //   triggers[note] = false;
    // }, 2000);
    if (note === key) {
      key = undefined;
    } else {
      key = note;
    }
  }
</script>

<Context bind:this={ctx} let:node>
  {#each keys as key}
    <button class="border-2 p-2 m-2" on:click={() => playNote(key)}>{key}</button>
    <!-- {#if triggers[key]}
      <Gain gain={0.2} dest={node} let:node>
        <Oscillator frequency={noteToHz(key)} dest={node} />
      </Gain>
    {/if} -->
  {/each}
  {#if key}
    <Gain gain={0.2} dest={node} let:node>
      <Oscillator frequency={{ value: noteToHz(key), endTime: 8 }} dest={node} />
    </Gain>
  {/if}
</Context>