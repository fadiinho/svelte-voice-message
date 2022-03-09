<script>
	import { onMount } from 'svelte';
	import Smile from '../components/Smile.svelte';
	import AudioPlayer, { playAudio } from '../components/AudioPlayer.svelte';

	let message = '';
	let voices = [];

	function clearValue() {
		message = '';
	}

	function messageError() {
		let text = document.querySelector('#text');

		function toggle() {
			text.classList.toggle('error');
		}

		if (text.classList.contains('error')) return;

		toggle();
		setTimeout(toggle, 2000);
	}

	async function fetchMessage() {
		const url = 'https://api.streamelements.com/kappa/v2/speech?';
		let select = document.querySelector('.voices');
		let voice = select.value;
		if (!message) {
			messageError();
			return;
		}

		let tts = await fetch(url + `voice=${voice}&text=${encodeURIComponent(message.trim())}`);

		if (!tts.ok) {
			// TODO: Make a warn
			return;
		}

		let mp3 = await tts.blob();

		let blobUrl = URL.createObjectURL(mp3);

		playAudio(blobUrl);
	}

	onMount(() => {
		voices = voicesArray;
	});
</script>

<main>
	<div class="container">
		<Smile />
		<h1>Teste Mensagem de Voz</h1>
		<textarea id="text" rows="10" cols="30" maxlength="134" bind:value={message} />
		<AudioPlayer />
		{#if voices}
			<select class="voices">
				{#each voices as voice}
					<option>{voice}</option>
				{/each}
			</select>
		{/if}
		<button on:click={fetchMessage} class="btn">Enviar</button>
	</div>
</main>

<style>
	@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500&display=swap');

	:root {
		--bg-color: #000;
		--primary-color: #040f16;
		--secondary-color: #0094c6;

		--error-color: #f00;
	}

	:global(*) {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}

	:global(body) {
		background-color: var(--bg-color);
		display: flex;
		justify-content: center;
		align-items: center;
		min-height: 100vh;
		font-family: 'Roboto', sans-serif;
	}

	.container {
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
	}

	h1 {
		color: var(--secondary-color);
	}

	#text {
		border-radius: 10px;
		outline: none;
		padding: 4px;
		resize: none;
		background-color: var(--primary-color);
		color: #fff;
		font-weight: 600;
		font-size: 1.2em;
		margin: 1.5rem 0;
		padding: 1em;
		transition: 0.5s;
		letter-spacing: 0.1em;
		border: 2px solid var(--secondary-color);
	}

	#text:focus {
		filter: drop-shadow(0px 0px 4px var(--secondary-color));
	}

	#text:global(.error) {
		filter: drop-shadow(0px 0px 4px var(--error-color));
		border: 2px solid var(--error-color);
		animation: error-animation 0.1s linear;
	}

	@keyframes error-animation {
		0% {
			transform: translateX(10px);
		}

		100% {
			transform: translateX(-10px);
		}
	}

	.btn {
		width: 5.5rem;
		height: 2rem;
		background-color: var(--primary-color);
		border: none;
		color: #fff;
		font-weight: 600;
		cursor: pointer;
		border: 2px solid var(--secondary-color);
		border-radius: 10px;
		transition: filter 0.5s;
	}

	.btn:hover {
		border: 2px solid var(--secondary-color);
		filter: drop-shadow(0px 0px 4px var(--secondary-color));
	}

	.voices {
		border: 2px solid var(--secondary-color);
		outline: none;
		border-radius: 8px;
		padding: 0.5rem;
		margin: 1.5rem 0;
		text-align: center;
		color: #fff;
		background-color: var(--primary-color);
		font-size: 1em;
		cursor: pointer;
		transition: 0.5s;
	}

	.voices:hover {
		filter: drop-shadow(0px 0px 4px var(--secondary-color));
	}

	/*
	.clear {
		position: relative;
		display: none;
		justify-content: center;
		align-items: center;
		width: 30px;
		height: 30px;
		cursor: pointer;
	}

	.clear::before {
		content: '';
		position: relative;
		width: 2px;
		height: 16px;
		background-color: var(--primary-color);
		transform: rotate(45deg) translateY(-1px);
	}

	.clear::after {
		content: '';
		position: relative;
		width: 2px;
		height: 16px;
		background-color: var(--primary-color);
		transform: rotate(-45deg) translateY(-1px);
	}
  */
</style>
