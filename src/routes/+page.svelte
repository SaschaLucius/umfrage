<script lang="ts">
	import type { Writable } from 'svelte/store';
	import { writable } from 'svelte/store';

	let numbers = Array.from({ length: 11 }, (_, i) => i);
	const writableArray: Writable<number[]> = writable([]);
	let printed = false;

	let avg: number;

	function clicked(i: number) {
		$writableArray = [...$writableArray, i];
		printed = false;
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

<div class="flex flex-col items-stretch justify-evenly">
	<div>
		<h1 class="text-3xl font-bold underline">
			Teilnehmerumfrage <br />({$writableArray.length} Teilnehmer)
		</h1>
		<h3>Danke für eure Teilnahme</h3>
	</div>

	<div class="button-container">
		{#each numbers as number, i}
			<div>
				<button
					on:click={() => clicked(i)}
					type="button"
					class="button variant-filled btn-icon btn-icon-xl">{number}</button
				>
			</div>
		{/each}
	</div>
	<br /><br />

	<div>
		{#if printed}
			<h1>Durchschnitt: {avg.toFixed(2)}</h1>
		{/if}
	</div>

	<div>
		<button
			type="button"
			data-umami-event="average-button-pressed"
			class="button2 variant-filled btn btn-xl"
			on:click={print}>Durchschnitt</button
		>
		<button
			type="button"
			data-umami-event="reset-button-pressed"
			class="button2 variant-filled btn btn-xl"
			on:click={reset}>Zurücksetzen</button
		>
	</div>
</div>

<style>
	.button {
		margin: 5px;
		font-size: 400%;
		height: 20vmin;
		width: 20vmin;
	}

	.button2 {
		margin: 5px;
		width: 46vmin;
	}

	.element-container {
		display: flex;
		flex-wrap: wrap;
		justify-content: space-evenly;
		flex-direction: column;
		align-items: center;
	}

	.button-container {
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
	}
</style>
