<script lang="ts">
	export let isMouseDown: boolean;

	const DEFAULT_BOARD_SIZE = 32;
	let boardSize = DEFAULT_BOARD_SIZE;
	let tiles = new Array(DEFAULT_BOARD_SIZE * DEFAULT_BOARD_SIZE).fill(false);
	let isRunning = false;

	const onTileMouseDown = (index: number) => {
		tiles[index] = !tiles[index];
	};

	const onMouseEnterTile = (index: number) => {
		if (isMouseDown) {
			tiles[index] = true;
		}
	};

	const step = () => {
		tiles = tiles.map((isAlive, index) => {
			// TODO: toss in some modulos
			const numberOfLiveNeighbors =
				Number(!!tiles[index - boardSize - 1]) +
				Number(!!tiles[index - boardSize]) +
				Number(!!tiles[index - boardSize + 1]) +
				Number(!!tiles[index - 1]) +
				Number(!!tiles[index + 1]) +
				Number(!!tiles[index + boardSize - 1]) +
				Number(!!tiles[index + boardSize]) +
				Number(!!tiles[index + boardSize + 1]);

			if (!isAlive) {
				return numberOfLiveNeighbors === 3;
			}

			if (numberOfLiveNeighbors < 2) {
				return !isAlive;
			}

			if (numberOfLiveNeighbors > 3) {
				return !isAlive;
			}

			if (numberOfLiveNeighbors === 2 || numberOfLiveNeighbors === 3) {
				return isAlive;
			}
		});
	};

	const stepTimeout = () => {
		if (isRunning) {
			step();
			setTimeout(stepTimeout, 75);
		}
	};

	const onStartClick = () => {
		isRunning = true;
		stepTimeout();
	};

	const onPauseClick = () => {
		isRunning = false;
	};

	const onClearClick = () => {
		tiles = new Array(boardSize * boardSize).fill(false);
	};

	const onBoardSizeChange = (
		e: Event & {
			currentTarget: EventTarget & HTMLInputElement;
		}
	) => {
		const newBoardSize = e.currentTarget.valueAsNumber;
		boardSize = newBoardSize;
		tiles = new Array(newBoardSize * newBoardSize).fill(false);

		isRunning = false;
	};

	const buttonStyles =
		'border-[1px] border-grey w-[160px] h-[40px] p-[5px] rounded hover:text-blue-600';
</script>

<div
	class="w-[70vh] h-[70vh] rounded mt-12"
	style="display: grid; box-shadow: 0px 1px 8px -1px rgb(0 0 0 / 20%), 
            0px 3px 24px -2px rgb(0 0 0 / 5%); grid-template-columns: repeat({boardSize}, minmax(0, 1fr))"
>
	{#each tiles as isAlive, index}
		<span
			class="border-slate-400 border-[1px] {isAlive ? 'bg-black' : ''}"
			on:mouseenter={() => onMouseEnterTile(index)}
			on:click={() => onTileMouseDown(index)}
		/>
	{/each}
</div>
<div class="flex mt-5">
	<button
		class={buttonStyles}
		style="box-shadow: #0062e3 0 0 3px"
		on:click={isRunning ? onPauseClick : onStartClick}
		>{#if isRunning} Pause {:else} Start {/if}</button
	>
	<button
		class={buttonStyles}
		style="box-shadow: #0062e3 0 0 3px; margin-left: 1rem"
		on:click={step}>Next</button
	>
	<button class={buttonStyles} style="box-shadow: #0062e3 0 0 3px; margin-left: 1rem">Clear</button>
</div>

<div
	class="pt-1 mt-4 rounded w-[512px] p-3"
	style="box-shadow: 0px 1px 8px -1px rgb(0 0 0 / 20%), 
            0px 3px 24px -2px rgb(0 0 0 / 5%)"
>
	<label for="board-size" class="form-label block"
		>{`Board Size: ${boardSize} x ${boardSize}`}</label
	>
	<input
		id="board-size"
		type="range"
		class="form-range appearance-none w-[75%] h-[6px] focus:outline-none focus:ring-0 focus:shadow-none bg-gray-300 cursor-pointer"
		default-value={DEFAULT_BOARD_SIZE}
		min={5}
		max={96}
		on:input={onBoardSizeChange}
	/>
</div>
