<script lang="ts">
  // svelte
  import { onMount } from 'svelte';
  import { onDestroy } from 'svelte';

  // app state
  let recording = false;
  let html = '';
  let recognition;

  const handleButton = () => {
    recording ? recognition?.stop() : recognition?.start();
    recording = !recording;
  };

  const onResult = (e: any) => {
    html = '';

    for (const r of e.results) {
      html += `${r[0].transcript}\n`;
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
      <div class="main__row">
        <button
          on:click={handleButton}
          id="button"
          class={recording ? 'button__recording' : ''}
          >{recording ? 'STOP RECORDING' : 'START RECORDING'}</button
        >
      </div>
      <div class="main__text">
        <textarea
          class="main__transcript"
          value={html || 'Your text will appear here as you speak.'}
          name="transcript"
        />
      </div>

      <div class="main__actions">
        <button>Copy Text</button>
        <button>Send via Email</button>
      </div>
    </section>
  </div>
</main>

<style type="text/scss">
  @import './styles/app.scss';
</style>
