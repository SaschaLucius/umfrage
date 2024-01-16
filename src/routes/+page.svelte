<script lang="ts">
	import type { Writable } from 'svelte/store';
	import { writable } from 'svelte/store';

	let numbers = Array.from({ length: 11 }, (_, i) => i);
	const writableArray: Writable<number[]> = writable([]);
	let printed = false;

	let avg: number;

	function clicked(i: number) {
		$writableArray = [...$writableArray, i];
	}

	function print() {
		const sum = $writableArray.reduce((a, b) => a + b, 0);
		avg = sum / $writableArray.length || 0;
		printed = true;
	}

	function reset() {
		$writableArray = [];
		printed = false;
	}
</script>

<h1>Teilnehmerumfrage {$writableArray.length}</h1>
<p>Danke für eure Teilnahme</p>

<div class="button-container">
	{#each numbers as number, i}
		<button
			on:click={() => clicked(i)}
			class="round"
			style="margin-right: {i % 3 === 2 ? '0' : '5px'};">{number}</button
		>
	{/each}
</div>

<button on:click={print}>Print</button>
<button on:click={reset}>Reset</button>

{#if printed}
	<h1>Durchschnitt: {avg.toFixed(2)}</h1>
{/if}

<style>
	.round {
		border-radius: 50%;
		margin-bottom: 5px;
		font-size: 50px;
	}

	.button-container {
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
	}
</style>
