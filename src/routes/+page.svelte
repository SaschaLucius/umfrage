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
		if ($writableArray.length > 0) {
			const sum = $writableArray.reduce((a, b) => a + b, 0);
			avg = sum / $writableArray.length || 0;
			printed = true;
		}
	}

	function reset() {
		$writableArray = [];
		printed = false;
	}
</script>

<h1>Teilnehmerumfrage ({$writableArray.length} Teilnehmer)</h1>
<h3>Danke für eure Teilnahme</h3>

<div class="button-container">
	{#each numbers as number, i}
		<div>
			<button
				on:click={() => clicked(i)}
				class="round"
				style="margin-right: {i % 3 === 2 ? '0' : '5px'};">{number}</button
			>
		</div>
	{/each}
</div>

<button style="font-size: 20px; border-radius: 10%;" on:click={print}>Durchschnitt</button>
<button style="font-size: 20px; border-radius: 10%;" on:click={reset}>Zurücksetzen</button>

{#if printed}
	<h1>Durchschnitt: {avg.toFixed(2)}</h1>
{/if}

<style>
	.round {
		border-radius: 50%;
		margin-bottom: 5px;
		font-size: 50px;
		height: 20vmin;
		width: 20vmin;
	}

	.button-container {
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
	}
</style>
