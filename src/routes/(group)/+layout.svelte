<script lang="ts">
	let { children } = $props();
</script>

<div class="@container pb-16 max-md:text-sm max-sm:text-xs">
	<div class="bg-tarred-800 text-tarred-100 grid w-full grid-cols-1 justify-items-center p-4">
		<a href="/">
			<span class="text-5xl font-bold">Tar's Site</span>
		</a>
		<span class="text-2xl">and stuff</span>
	</div>
	<div class="grid w-full justify-center">
		<div class="w-[100cqw] max-w-[100em] px-8 pt-8">
			<div class="h-full w-full">
				{@render children()}
			</div>
		</div>
	</div>
</div>
