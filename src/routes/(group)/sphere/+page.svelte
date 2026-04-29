<script lang="ts">
	import ButtonComponent from '$lib/ButtonComponent.svelte';
	import TerminalComponent from '$lib/TerminalComponent.svelte';
	import katex from 'katex';
</script>

<div class="grid w-full gap-2">
	<TerminalComponent title="what is it?">
		<p>one day i was in calc class learning about using integrals to find the volume of shapes,</p>
		<p>i thought that was pretty dang cool.</p>
		<p></p>
		<p>but i wanted more</p>
		<p>so i wondered, 'how could i expand on this?'</p>
		<p></p>
		<p></p>
		<p>we had just found the volume of a sphere, so i wondered 'could i find the surface area?'</p>
		<p></p>
		<p>i tried to think of a way that i could use the integrals that we'd been using for it,</p>
		<p>and my first idea was to add up a bunch of tubes</p>
		<p></p>
		<p>
			i decided i would just estimate a hemisphere and double it at the end to get the full sphere.
		</p>
	</TerminalComponent>

	<ButtonComponent class="iframe-button">
		<iframe
			id="calculator"
			sandbox="allow-same-origin allow-scripts"
			src="https://www.desmos.com/3d/8ed5yaouvb?embed"
			title="3D Calculator for Sphere Visualization"
		></iframe>
	</ButtonComponent>

	<TerminalComponent title="but...">
		<p>i checked what this would approach as i added more and more layers...</p>
		<p>and it wasnt right :(</p>
		<p></p>
		<p>this confused me for a while</p>
		<p>but eventually i understood why this is wrong</p>
		<p></p>
		<p></p>
		<p>to explain this, lets take a simple example</p>
		<p>a diagonal line</p>
		<p></p>
		<p>lets say i want to estimate the length of the line</p>
		<p>
			in order to do this estimation in the same way as i did the sphere, we'll be counting up
			vertical line segments
		</p>
	</TerminalComponent>

	<ButtonComponent class="iframe-button">
		<iframe
			id="calculator"
			sandbox="allow-same-origin allow-scripts"
			src="https://www.desmos.com/calculator/m9onpcju5j?embed"
			class="desmos-embed right"
			title="Desmos Calculator for Line Visualization"
		></iframe>
	</ButtonComponent>

	<TerminalComponent title="so!">
		<p>so</p>
		<p>you can see that the line segments dont add up to the right length</p>
		<p>even if there were 999999 segments, it would add up to the same (wrong) length</p>
		<p></p>
		<p></p>
		<p>
			in order to remedy this, i started thinking about ways i could approximate the surface of a
			sphere using only polygons, making sure they all fit together to prevent the problem that
			happened with the disjointed tubes not adding up to the right total
		</p>
		<p></p>
		<p>the first thing that came to mind were dice</p>
		<p>people have designed dice with anywhere from 4 to 100 sides</p>
		<p></p>
		<p>
			unfortunately, the farther i looked into it, the more i realized that dice are extremely
			complicated
		</p>
		<p></p>
		<p>you cant just follow a simple pattern to increase the sides of a die</p>
		<p>youd have to find a whole new pattern for every increase in sides</p>
		<p></p>
		<p></p>
		<p>
			not feeling like finding an infinite number of unique patterns, i turned to somethign else...
		</p>
		<p></p>
		<p>trapezoids!</p>
	</TerminalComponent>

	<ButtonComponent class="iframe-button">
		<iframe
			id="calculator"
			sandbox="allow-same-origin allow-scripts"
			src="https://www.desmos.com/calculator/cdfpnap8yr?embed"
			class="desmos-embed right"
			title="Desmos Calculator for Trapezoid Visualization"
		></iframe>
	</ButtonComponent>

	<TerminalComponent title="trapezoids!">
		<p>by splitting the sphere into {@html katex.renderToString('n')} layers</p>
		<p>and spacing {@html katex.renderToString('n')} points evenly on each layer,</p>
		<p>then connecting the points on each layer along the peremeter,</p>
		<p>and each point to the corresponding point above it,</p>
		<p></p>
		<p>
			you get a shape that approaches a sphere as {@html katex.renderToString(
				'n \\rightarrow \\infin'
			)}!
		</p>
		<p></p>
		<p>
			not long after, i developed a formula that represents the trapezoids added up as {@html katex.renderToString(
				'n \\rightarrow \\infin'
			)}
		</p>
		<p></p>
		<p>now all thats left is simplification...</p>
	</TerminalComponent>

	<TerminalComponent title="darn :/">
		<p>well that took a while to do</p>
		<p>im going to skip past the simplification process</p>
		<p></p>
		<p>it took me an entire year of working on and off with a couple of my friends</p>
		<p></p>
		<p>if you want to see the entire process click</p>
		<p>here</p>
	</TerminalComponent>

	<TerminalComponent title="finally">
		<p>so..</p>
		<p>
			after SO much simplification i eventually got that huge equation up there down to something
			managable
		</p>
		<p></p>
		<p>turns out that it just equals {@html katex.renderToString('2 \\pi r^{2}')}</p>
		<p>which is half a sphere, exactly correct</p>
		<p></p>
		<p>tada!</p>
	</TerminalComponent>

	<ButtonComponent>
		<div class="grid grid-cols-1 grid-rows-1">
			<div class="col-start-1 row-start-1 h-full w-full overflow-x-scroll overflow-y-hidden">
				{@html katex.renderToString(
					String.raw`\lim_{ n \to\infty}\left(\displaystyle\sum_{i=0}^{n-1}\left(n*\sqrt{\left(\left(\dfrac{\left|-\left(\sqrt{r^{2}-\left(\left(i+1\right)\cdot\dfrac{r}{n}\right)^{2}}\right)\left(\dfrac{-\sin\left(\dfrac{2\pi}{n}\right)}{1-\cos\left(\dfrac{2\pi}{n}\right)}\right)+\left(\sqrt{r^{2}-\left(i\cdot\dfrac{r}{n}\right)^{2}}\right)\left(\dfrac{-\sin\left(\dfrac{2\pi}{n}\right)}{1-\cos\left(\dfrac{2\pi}{n}\right)}\right)\right|}{\sqrt{1+\left(\dfrac{-\sin\left(\dfrac{2\pi}{n}\right)}{1-\cos\left(\dfrac{2\pi}{n}\right)}\right)^{2}}}\right)^2+\left(\dfrac{r}{n}\right)^{2}\right)}\cdot\dfrac{\left(\sqrt{2\left(\sqrt{r^{2}-\left(i\cdot\dfrac{r}{n}\right)^{2}}\right)^{2}-2\left(\sqrt{r^{2}-\left(i\cdot\dfrac{r}{n}\right)^{2}}\right)^{2}\cos\left(\dfrac{2\pi}{n}\right)}\right)+\left(\sqrt{2\left(\sqrt{r^{2}-\left(\left(i+1\right)\cdot\dfrac{r}{n}\right)^{2}}\right)^{2}-2\left(\sqrt{r^{2}-\left(\left(i+1\right)\cdot\dfrac{r}{n}\right)^{2}}\right)^{2}\cos\left(\dfrac{2\pi}{n}\right)}\right)}{2}\right)\right)`
				)}
			</div>
			<div
				class="pointer-events-none col-start-1 row-start-1 flex h-full w-full items-center justify-end"
				aria-hidden="true"
			>
				<div class="animate-bounce-r">
					<svg class="size-7" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
						<path
							d="M8 5L16 12L8 19"
							stroke="currentColor"
							stroke-width="2.5"
							stroke-linecap="round"
							stroke-linejoin="round"
						/>
					</svg>
				</div>
			</div>
		</div>
	</ButtonComponent>

	<TerminalComponent title="darn :/">
		<p>well that took a while to do</p>
		<p>im going to skip past the simplification process</p>
		<p></p>
		<p>it took me an entire year of working on and off with a couple of my friends</p>
		<p></p>
		<p>if you want to see the entire process click</p>
		<a href="https://example.com">here</a>
	</TerminalComponent>
</div>

<style>
	:global(.terminal-component) {
		width: min(90%, 60rem);
	}

	:global(.iframe-button) {
		width: min(90%, 60rem);
		height: 30rem;

		justify-self: end;
	}

	iframe {
		width: 100%;
		height: 100%;
		border-radius: inherit;
		background-color: var(--color-tarblue-700);
	}
</style>
