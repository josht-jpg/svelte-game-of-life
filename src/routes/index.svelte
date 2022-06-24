<script>
	import Board from '../components/Board.svelte';

	let isMouseDown = false;

	const onMouseDown = () => {
		isMouseDown = true;
	};

	const onMouseUp = () => {
		isMouseDown = false;
	};
</script>

<svelte:head>
	<title>Game of Life</title>
</svelte:head>
<div
	on:mousedown={onMouseDown}
	on:mouseup={onMouseUp}
	class="flex w-full flex-1 flex-col items-center justify-center px-20 text-center"
>
	<Board {isMouseDown} />
</div>
