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
			// .nodeCanvasObject((node, ctx, globalScale) => {
			// 					const label = node.id;
			// 					const fontSize = 18/globalScale;
			// 					ctx.font = `${fontSize}px Sans-Serif`;
			// 					const textWidth = ctx.measureText(label).width;
			// 					const bckgDimensions = [textWidth, fontSize].map(n => n + fontSize * 0.2); // some padding
			//
			// 					ctx.fillStyle = 'rgba(255, 255, 255, 0.8)';
			// 					ctx.fillRect(node.x - bckgDimensions[0] / 2, node.y - bckgDimensions[1] / 2, ...bckgDimensions);
			//
			// 					ctx.textAlign = 'center';
			// 					ctx.textBaseline = 'middle';
			// 					ctx.fillStyle = 'rgba(255, 0, 0, 0.8)';
			// 					ctx.fillText(label, node.x, node.y);
			//
			// 					node.__bckgDimensions = bckgDimensions; // to re-use in nodePointerAreaPaint
			// 				})
		Graph.d3Force('charge').strength(-120)
		})
	});
</script>

<main>
	<div bind:this={canvasDom} />
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		margin: 0 auto;
	}


	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
