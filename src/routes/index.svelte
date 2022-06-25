<script lang="ts">
	import Board from '../components/Board.svelte';

	let isMouseDown = false;
	let isShiftKeyDown = false;

	const onMouseDown = () => {
		isMouseDown = true;
	};

	const onMouseUp = () => {
		isMouseDown = false;
	};

	const onKeyDown = (e: KeyboardEvent) => {
		if (e.key === 'Shift') {
			isShiftKeyDown = true;
		}
	};

	const onKeyUp = (e: KeyboardEvent) => {
		if (e.key === 'Shift') {
			isShiftKeyDown = false;
		}
	};
</script>

<svelte:head>
	<title>Game of Life</title>
</svelte:head>

<svelte:window on:keydown={onKeyDown} on:keyup={onKeyUp} />

<div
	on:mousedown={onMouseDown}
	on:mouseup={onMouseUp}
	class="flex w-full flex-1 flex-col items-center justify-center px-20 text-center"
>
	<Board {isMouseDown} {isShiftKeyDown} />
</div>
