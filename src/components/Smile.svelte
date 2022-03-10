<script>
	import { onMount } from 'svelte';

	const handleClick = (e) => {
		document.querySelector('.face').classList.toggle('happy');
		document.querySelector('.face').classList.toggle('sad');
	};
</script>

Mind Linda
<div class="face sad">
	<div class="eyes">
		<div class="eye" id="eye1" />
		<div class="eye" id="eye2" />
	</div>
	<div class="mouth" />
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
		position: relative;
		display: flex;
		justify-content: center;
		width: 8px;
		height: 8px;
		background-color: #000;
		border-radius: 50%;
		overflow: hidden;
		transition: 0.2s;
	}

	.face:global(.happy) > .eyes > #eye1,
	.face:global(.happy) > .eyes > #eye2 {
		transform: scale(2, 2);
	}

	.face:global(.happy) > .eyes > #eye1::before,
	.face:global(.happy) > .eyes > #eye2::before {
		content: '';
		position: absolute;
		width: 50%;
		height: 50%;

		border-radius: 10px;
		background-color: #fff;
	}
	.face:global(.happy) > .eyes > #eye1::before {
		right: 5%;
	}

	.face:global(.happy) > .eyes > #eye2::before {
		left: 5%;
	}

	.face:global(.sad) > .eyes > #eye1,
	.face:global(.sad) > .eyes > #eye2 {
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.face:global(.sad) > .eyes > #eye1::before,
	.face:global(.sad) > .eyes > #eye2::before {
		content: '';
		position: absolute;
		width: 50%;
		height: 50%;

		border-radius: 10px;
		background-color: #fff;
	}

	.mouth {
		display: flex;
		position: relative;
		justify-content: center;
		width: 30px;
		height: 8px;
		background-color: #000;
		transition: 0.1s;
		transform: translateY(4px);
	}

	.face:global(.happy) > .mouth {
		border-bottom-right-radius: 10px;
		border-bottom-left-radius: 10px;
		transform: scale(1, 1.5) translateY(2px);
	}

	.face:global(.sad) > .mouth {
		border-top-right-radius: 10px;
		border-top-left-radius: 10px;
	}

	/* Tongue */
	.face:global(.happy) > .mouth::before {
		content: '';
		position: absolute;
		width: 12px;
		top: 80%;
		height: 12px;
		background-color: #f00;
		border-radius: 0 0 40% 40%;
		animation: openTongue 0.1s;
	}

	.face:global(.sad) > .mouth::before {
		content: '';
		position: absolute;
		width: 12px;
		top: 80%;
		height: 0;
		background-color: #f00;
		border-radius: 0 0 40% 40%;
		animation: closeTongue 0.1s;
		z-index: 99;
	}

	@keyframes openTongue {
		from {
			height: 0px;
		}
		to {
			height: 12px;
		}
	}

	@keyframes closeTongue {
		from {
			height: 12px;
		}
		to {
			height: 0;
		}
	}

	/* Inset Radius */
	.face:global(.sad) > .mouth::after,
	.face:global(.happy) > .mouth::after {
		content: '';
		position: absolute;
		width: 100%;
		height: 20%;
		background-color: var(--secondary-color);
	}

	.face:global(.sad) > .mouth::after {
		bottom: -10%;
		border-top-right-radius: 100%;
		border-top-left-radius: 100%;
	}

	.face:global(.happy) > .mouth::after {
		top: -10%;
		border-bottom-right-radius: 100%;
		border-bottom-left-radius: 100%;
	}
</style>
