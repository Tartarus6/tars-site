<script lang="ts">
	import ButtonComponent from './ButtonComponent.svelte';
	import type { Snippet } from 'svelte';

	interface Props {
		title: string;
		class?: string;
		children?: Snippet;
	}

	let props: Props = $props();
	const rootClass = $derived(`terminal-component w-full pb-2 ${props.class ?? ''}`);
</script>

<div class={rootClass}>
	<ButtonComponent>
		<div class="grid w-full grid-cols-1">
			<div class="mb-2 grid grid-cols-[auto_1fr]">
				<span class="mr-6 text-xl font-extrabold select-none">>_</span>
				<span class="text-lg">{props.title}</span>
			</div>
			<div class="bg-tarblue-800 terminal-component-content w-full rounded-2xl p-2">
				{#if props.children}
					{@render props.children()}
				{/if}
			</div>
		</div>
	</ButtonComponent>
</div>

<style>
	.terminal-component-content {
		color: var(--color-tarblue-100, #e2e8f0);
	}

	:global(.terminal-component-content > *) {
		margin: 0;
		position: relative;
		padding-left: 1rem;
	}

	/* allow for empty lines to be rendered */
	:global(.terminal-component-content > *:empty::after) {
		content: '\200B'; /* add a zero width space so that the line automatically sizes properly */
	}

	:global(.terminal-component-content > *::before) {
		content: '>';
		position: absolute;
		left: 0;
		top: 0;
		font-weight: bold;
		color: rgb(34 197 94); /* text-green-500 */
		user-select: none;
		font-size: 1rem;
	}
</style>
