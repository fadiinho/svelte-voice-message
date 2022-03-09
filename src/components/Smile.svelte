<script>
	import { onMount } from 'svelte';

	const handleClick = (e) => {
		document.querySelector('.mouth').classList.toggle('happy');
		document.querySelector('.mouth').classList.toggle('sad');
	};
</script>

<div class="face">
	<div class="eyes">
		<span class="eye" id="eye1" />
		<span class="eye" id="eye2" />
	</div>
	<span class="mouth sad" />
	<div on:click={handleClick} class="face-placeholder" />
</div>

<style>
	.face {
		display: flex;
		justify-content: center;
		align-items: center;
		flex-direction: column;
		width: 80px;
		height: 80px;
		background-color: var(--secondary-color);
		border: none;
		border-radius: 50%;
		gap: 14px;
		cursor: pointer;
	}

	.face:hover {
		filter: drop-shadow(0 0 20px var(--secondary-color));
	}

	.face-placeholder {
		position: absolute;
		width: 80px;
		height: 80px;
		border-radius: 50%;
		z-index: 9999;
	}

	.eyes {
		display: flex;
		justify-content: space-around;
		align-items: center;
		width: 100%;
	}

	.eye {
		display: block;
		width: 8px;
		height: 8px;
		background-color: #000;
		border-radius: 50%;
	}

	.mouth {
		display: block;
		width: 30px;
		height: 8px;
		background-color: #000;
		transition: 0.1s;
		transform: translateY(4px);
	}

	.mouth:global(.happy) {
		border-bottom-right-radius: 10px;
		border-bottom-left-radius: 10px;
	}

	.mouth:global(.sad) {
		border-top-right-radius: 10px;
		border-top-left-radius: 10px;
	}
</style>
