<script lang="ts">
  // svelte
  import { onMount } from 'svelte';
  import { onDestroy } from 'svelte';

  // app state
  let recording = false;
  let html = [];
  let recognition;

  const handleButton = () => {
    recording ? recognition?.stop() : recognition?.start();
    recording = !recording;
  };

  const onResult = (e: any) => {
    html = [];

    for (const r of e.results) {
      html.push(`<p>${r[0].transcript}</p>`);
    }
  };

  onMount(() => {
    // init SpeechRecognition API
    const SpeechRecognition =
      window.SpeechRecognition || window.webkitSpeechRecognition;
    recognition = new SpeechRecognition();

    // recognition options
    recognition.continuous = true;
    recognition.interimResults = true;
  });

  onMount(() => recognition.addEventListener('result', onResult));

  onDestroy(() => recognition.removeEventListener('result', onResult));
</script>

<main>
  <div class="main__content">
    <header>
      <h1>Vocalise</h1>
      <h2>Live Speech to Text Converter</h2>
    </header>
    <section>
      <button on:click={handleButton} id="button"
        >{recording ? 'STOP RECORDING' : 'START RECORDING'}</button
      >
      <div class="main__text">
        {@html html}
      </div>

      <p>
        {recording ? 'RECORDING...' : ''}
      </p>
    </section>
  </div>
</main>

<style type="text/scss">
  @import './styles/app.scss';
</style>
