<script lang="ts">
	import { onMount } from 'svelte';
	import { decode } from 'blurhash';

	export let width: number;
	export let height: number;
	export let hash: string;
	export let src: string;
	export let imgAlt: string;
	export let className: string;
	export let style: string = "";

	let canvas: HTMLCanvasElement;
	let isImageLoaded = false;

	onMount(() => {
		const image = new Image();
		image.src = src;

		const pixels = decode(hash, width, height);
		const ctx = canvas.getContext('2d')!;
		const imageData = ctx.createImageData(width, height);

		imageData.data.set(pixels);
		ctx.putImageData(imageData, 0, 0);

		image.onload = () => {
			isImageLoaded = true;
		};
	});
</script>

{#if isImageLoaded}
	<img {src} class={className} alt={imgAlt} {style} />
{:else}
	<canvas class={className} bind:this={canvas} {width} {height} {style} />
{/if}
