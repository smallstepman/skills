<script lang="ts">
	import ForceGraph3D from '3d-force-graph';
	import SpriteText from 'three-spritetext';
	import { onMount } from "svelte";

	let canvasDom;//: HTMLElement;

	onMount(() => {
		fetch('./dataset.json').then(res => res.json()).then(data => {
			const Graph = ForceGraph3D()(canvasDom)
				.graphData(data)
				.nodeId('id')
				.nodeVal('val')
				.nodeLabel('id')
				.nodeAutoColorBy('group')
				.linkSource('source')
				.linkTarget('target')
				.linkColor('rgba(255, 0, 0, 0.8)')
				.nodeThreeObject(node => {
					const sprite = new SpriteText(node.id);
					sprite.material.depthWrite = false; // make sprite background transparent
					sprite.color = node.color;
					sprite.textHeight = 'big' in node ? 17 : 8 ;
					return sprite;
				})
		Graph.d3Force('charge').strength(-120)
		})
	});
</script>

<main>
	<div bind:this={canvasDom} />
</main>
