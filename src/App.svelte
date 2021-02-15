<script lang="ts">
  import { onMount } from 'svelte';
  import Tailwind from './Tailwind.svelte';
  import AudioContext from './audio/AudioContext.svelte';
  import Gain from './audio/Gain.svelte';
  import Oscillator from './audio/Oscillator.svelte';
  import BiquadFilter from './audio/BiquadFilter.svelte';

  let ctx: window.AudioContext;

  function start() {
    ctx.resume();
  }

  function stop() {
    ctx.suspend();
  }

  let oscTypes: OscillatorType[] = ['sine', 'sawtooth', 'square', 'triangle'];
  let filterTypes: BiquadFilterType[] = ['lowpass', 'bandpass', 'highpass', 'allpass', 'lowshelf', 'highshelf', 'notch', 'peaking']

  let filterType = filterTypes[0];
  let filterFreq = 440;

  let carrierType = oscTypes[0];
  let modulatorType = oscTypes[0];
  let gain = 0.1;
  let frequency = 440;
  let modGain = 5;
  let modFreq = 20;

  onMount(() => {
    ctx.suspend();
  });
</script>

<Tailwind />

<div class="p-4">
  <h1 class="font-bold text-3xl">FM Svnth</h1>

  <div class="flex flex-wrap">
    <div class="border-2 border-gray-900 m-4 p-8">
      <label class="block" for="mastergain">Master Gain</label>
      <input id="mastergain" type="range" min={0} max={1} step={0.01} bind:value={gain} />
      <span class="text-center bg-yellow-50 w-10 inline-block">{gain}</span>
    </div>

    <div class="border-2 border-gray-900 m-4 p-8">
      <h2 class="font-bold text-xl">Filter</h2>
      <label for="filterType">Type</label>
      <select id="filterType" bind:value={filterType}>
        {#each filterTypes as type}
          <option value={type}>{type}</option>
        {/each}
      </select>
      <div>
        <label for="filterFreq">Frequency</label>
        <input id="filterFreq" type="range" min={20} max={2000} bind:value={filterFreq} />
        <span class="text-center bg-yellow-50 w-10 inline-block">{filterFreq}</span>
      </div>
    </div>

    <div class="border-2 border-gray-900 m-4 p-8">
      <h2>Carrier</h2>
      <label for="carrierType">Oscillator</label>
      <select id="carrierType" bind:value={carrierType}>
        {#each oscTypes as type}
          <option value={type}>{type}</option>
        {/each}
      </select>
      <div>
        <label class="block" for="frequency">Frequency</label>
        <input id="frequency" type="range" min={20} max={1000} bind:value={frequency} />
        <span class="text-center bg-yellow-50 w-10 inline-block">{frequency}</span>
      </div>
    </div>

    <div class="border-2 border-gray-900 m-4 p-8">
      <h2>Modulator</h2>
      <div>
        <label class="block" for="amp">Modulator amplitude</label>
        <input id="amp" type="range" min={0} max={200} bind:value={modGain} />
        <span>{modGain}</span>
      </div>

      <label for="carrierType">Oscillator</label>
      <select id="carrierType" bind:value={modulatorType}>
        {#each oscTypes as type}
          <option value={type}>{type}</option>
        {/each}
      </select>

      <div>
        <label class="block" for="modfreq">Modulator frequency</label>
        <input id="modfreq" type="range" min={0} max={1000} bind:value={modFreq} />
        <span class="text-center bg-yellow-50 w-10 inline-block">{modFreq}</span>
      </div>
    </div>

    <div>
      <button
        class="bg-gray-900 text-white px-8 py-4 m-12 font-bold"
        on:click={start}
      >
        Start
      </button>

      <button
        class="bg-gray-900 text-white px-8 py-4 m-12 font-bold"
        on:click={stop}
      >
        Stop
      </button>
    </div>
  </div>
</div>

<AudioContext bind:this={ctx} let:node>
  <BiquadFilter type={filterType} frequency={filterFreq} dest={node} let:node>
    <Gain {gain} dest={node} let:node>
      <Oscillator frequency={frequency} type={carrierType} dest={node} let:node>
        <Gain gain={modGain} dest={node.frequency} let:node>
          <Oscillator frequency={modFreq} type={modulatorType} dest={node} />
        </Gain>
      </Oscillator>
    </Gain>
  </BiquadFilter>
</AudioContext>
