<script lang="ts">
	import CardComponent from '$lib/CardComponent.svelte';
	import TerminalComponent from '$lib/TerminalComponent.svelte';

	import IconProjectEuler from '$lib/icon_project_euler.svg?raw';
	import IconSvelte from '$lib/icon_svelte.svg?raw';
	import IconDesmos from '$lib/icon_desmos.svg?raw';
	import IconSlimepedia from '$lib/iconSlimePink.svg?raw';
	import IconHiby from '$lib/icon_hiby.svg?raw';
	import BingoIconComponent from '$lib/BingoIconComponent.svelte';
	import type { Component } from 'svelte';

	type Cards = {
		title: string;
		content: string;
		icon: string | { component: Component; props: any };
		href: string;
	}[];

	const cards: Cards = [
		{
			title: 'Slimepedia',
			content:
				"I made a very, VERY, faithful recreation of the Slimepedia from the game Slime Rancher 2. It acts as a wiki built into the game, and now it's built into my website. (Sound Recommended)",
			icon: IconSlimepedia,
			href: 'https://slimepedia.tartarus6.com'
		},
		{
			title: 'HiBy OS Crack',
			content:
				"HiBy is a company that makes modern MP3 players. This project is working on cracking the firmware of HiBy's linux devices as well as developing custom modded firmware for them.",
			icon: IconHiby,
			href: 'https://github.com/hiby-modding'
		},
		{
			title: 'Sphere Surface Area',
			content:
				'I have spent a very very long time proving the surface of a sphere using only polygons. I ended up using a bunch of trapezoids, and proving what the surface area approaches. This page is my ramblings explaining how I did it, including cool desmos graphs.',
			icon: IconDesmos,
			href: '/sphere'
		},
		{
			title: 'Bingo Problem',
			content:
				'An interesting math problem that I have been working on a closed solution for, and have yet to succeed. Combinatorics is neat. This page has some visuals that hopefully help make the problem intuitive enough.',
			icon: {
				component: BingoIconComponent,
				props: {
					interactive: false,
					boardState: [
						false,
						true,
						true,
						true,
						true,
						true,
						true,
						true,
						false,
						true,
						true,
						false,
						true,
						true,
						true,
						true,
						true,
						false,
						true,
						true,
						true,
						true,
						true,
						true,
						false
					]
				}
			},
			href: '/bingo-problem'
		},
		{
			title: 'Bingo',
			content:
				"Right now, it's just got one board which has catchphrases from someone. But I might add more.",
			icon: {
				component: BingoIconComponent,
				props: {}
			},
			href: '/bingo'
		},
		{
			title: 'Notes App',
			content:
				'I have been unhappy with every notes app I have tried. So I made my own. At the moment its just an open test site, but I plan to build it into an actual app.',
			icon: IconSvelte,
			href: 'https://notes.tartarus6.com'
		},
		{
			title: 'Project Euler',
			content:
				'Project Euler is a website with a bunch of programming challenges. Solving them is how I learned to program. Here is a page that walks through how to solve a few, as a resource to help learning programming.',
			icon: IconProjectEuler,
			href: '/project-euler'
		}
	];
</script>

<div class="nav-card mx-auto mb-32 grid grid-cols-1 place-items-center">
	<TerminalComponent title="hi!">
		<p>hello</p>
		<p>my name is Tar!</p>
		<p>im a programmer, and i like to make things</p>
		<p></p>
		<p>isnt this quite the website?</p>
		<p>i made it using SvelteKit and tailwindcss</p>
	</TerminalComponent>
	<TerminalComponent title="website?">
		<p>yeah!</p>
		<p>this website is sort of like a resume for me</p>
		<p></p>
		<p>below are some cool projects i've done</p>
	</TerminalComponent>
</div>

<div class="grid w-full grid-cols-1">
	{#each cards as card, i}
		<div class="nav-card {i % 2 === 0 ? '' : 'justify-self-end'}">
			<CardComponent
				flipped={i % 2 === 1 ? 'true' : 'false'}
				title={card.title}
				content={card.content}
				href={card.href}
			>
				{#if typeof card.icon === 'string'}
					{@html card.icon}
				{:else if typeof card.icon === 'object' && 'component' in card.icon}
					<svelte:component this={card.icon.component} {...card.icon.props} />
				{/if}
			</CardComponent>
		</div>
	{/each}
</div>

<style>
	.nav-card {
		width: min(90%, 60rem);
	}
</style>
