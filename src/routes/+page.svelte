<script lang="ts">
	import type { TurnstileTheme, TurnstileSize } from '$lib';
	import { enhance } from '$app/forms';
	import { Turnstile } from '$lib';

	export let form;

	let secretKey = '1x0000000000000000000000000000000AA';
	let siteKey = '1x00000000000000000000AA';
	let theme: TurnstileTheme = 'auto';
	let size: TurnstileSize = 'normal';

	let reset: () => void | undefined;
</script>

<section class="row">
	<label>
		Demo Site Key type

		<select bind:value={siteKey}>
			<option value="1x00000000000000000000AA">Always Pass</option>
			<option value="2x00000000000000000000AB">Always Block</option>
			<option value="3x00000000000000000000FF">
				Force interactive challenge
			</option>
		</select>
	</label>

	<label>
		Demo Secret Key type

		<select bind:value={secretKey}>
			<option value="1x0000000000000000000000000000000AA">
				Always Pass
			</option>

			<option value="2x0000000000000000000000000000000AA">
				Always Block
			</option>

			<option value="3x0000000000000000000000000000000AA">
				Token already spent error
			</option>
		</select>
	</label>

	<label>
		Theme

		<select bind:value={theme}>
			<option value="auto">Auto</option>
			<option value="dark">Dark</option>
			<option value="light">Light</option>
		</select>
	</label>

	<label>
		Size

		<select bind:value={size}>
			<option value="normal">Normal</option>
			<option value="flexible">Flexible</option>
			<option value="compact">Compact</option>
		</select>
	</label>
</section>

<section>
	<form method="POST" use:enhance>
		<Turnstile {size} {theme} {siteKey} bind:reset />
		<input type="hidden" name="secret" bind:value={secretKey} />

		<div class="row">
			<button>Validate</button>
			<button type="button" on:click={() => reset?.()}>Reset</button>
		</div>
	</form>

	<p style="margin-top: 8px;">
		{#if form}
			{form.error ? `Error: ${form.error}` : 'Success'}
		{/if}
	</p>
</section>

<style lang="scss">
	label {
		display: flex;
		flex-direction: column;
		gap: 8px;
	}

	form {
		width: 50%;
		max-width: 600px;
	}
</style>
