<script lang="ts">
  import Tailwind from './Tailwind.svelte';
  import AudioContext from './AudioContext.svelte';
  import Gain from './Gain.svelte';
  import Oscillator from './Oscillator.svelte';
  import BiquadFilter from './BiquadFilter.svelte';

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
</script>

<Tailwind />

<div class="p-4">
  <h1>FM Synth</h1>
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

  <div>
    <label class="block" for="mastergain">Master Gain</label>
    <input id="mastergain" type="range" min={0} max={1} step={0.01} bind:value={gain} />
    <span>{gain}</span>
  </div>

  <div>
    <h2>Filter</h2>
    <label for="filterType">Type</label>
    <select id="filterType" bind:value={filterType}>
      {#each filterTypes as type}
        <option value={type}>{type}</option>
      {/each}
    </select>
    <label for="filterFreq">Frequency</label>
    <input id="filterFreq" type="range" min={20} max={2000} bind:value={filterFreq} />
  </div>

  <div>
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
      <span>{frequency}</span>
    </div>
  </div>

  <div>
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
      <span>{modFreq}</span>
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
