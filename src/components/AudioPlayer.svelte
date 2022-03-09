<script context="module">
	let progressBar;
	let innerProgressBar;
	let source;
	let audio;

	export function setProgress(e = null, p = 0) {
		if (p === 0 && e !== null) {
			let time = (e.target.currentTime / e.target.duration) * 250;
			let percentage = (time / 250) * 100;
			innerProgressBar.style.width = percentage + '%';
			return;
		}

		innerProgressBar.style.width = p + '%';
		return;
	}

	export function playAudio(url) {
		if (!url) return;

		source.setAttribute('src', url);
		setProgress(null, 0);

		audio.pause();
		audio.load();
		audio.play();
	}
</script>

<script>
	import { onMount } from 'svelte';
	let paused = true;

	function handleClick() {
		// TODO: Make a warn if not set
		if (!audio.currentSrc) return;
		if (!paused) {
			paused = true;
			audio.play();
		} else {
			paused = false;
		}
	}

	onMount(() => {
		progressBar = document.querySelector('div.progress');
		innerProgressBar = document.querySelector('div.inner-progress');
		source = document.querySelector('source#source');
		audio = document.querySelector('audio#audio');
	});
</script>

<div class="audio">
	<audio bind:paused on:timeupdate={(e) => setProgress(e)} id="audio">
		<source id="source" type="audio/wav" />
	</audio>
	<div class="progress">
		<div class="inner-progress" />
	</div>
	<div on:click={handleClick} class="controls">
		{#if paused}
			<div class="play" />
		{:else}
			<div class="pause">
				<div />
				<div />
			</div>
		{/if}
	</div>
</div>

<style>
	.audio {
		display: flex;
		justify-content: space-evenly;
		align-items: center;
		width: 250px;
		height: 50px;
		background-color: var(--primary-color);
		border: 2px solid var(--secondary-color);
		border-radius: 10px;
		padding-left: 0.2em;
		transition: 0.5s;
	}

	.audio:hover {
		filter: drop-shadow(0 0 4px var(--secondary-color));
	}

	.progress {
		width: 80%;
		height: 10px;
		background-color: #fff;
		border-radius: 8px;
	}

	.inner-progress {
		width: 0%;
		height: 100%;
		background-color: var(--secondary-color);
		border-radius: 8px;
		filter: drop-shadow(0 0 4px var(--secondary-color));
	}

	.controls {
		display: flex;
		justify-content: space-around;
		align-items: center;
		width: 20px;
		height: 20px;
		cursor: pointer;
	}

	.pause {
		display: flex;
		justify-content: space-around;
		align-items: center;
		width: 100%;
		height: 100%;
		pointer-events: none;
		transition: 0.5s;
	}

	.pause > div {
		width: 4px;
		height: 15px;
		background-color: var(--secondary-color);
	}

	.play {
		pointer-events: none;
		width: 0;
		height: 0;
		border-top: 8px solid transparent;
		border-bottom: 8px solid transparent;

		border-left: 14px solid var(--secondary-color);
		transition: 0.5s;
	}
</style>
