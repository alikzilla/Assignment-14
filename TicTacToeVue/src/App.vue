<script setup>
import { ref, computed, toDisplayString } from 'vue'

const player = ref('X');
const board = ref([
  ['', '', ''],
  ['', '', ''],
  ['', '', '']
]);
let scoreX = 0;
let scoreO = 0;

const CalculateWinner = (board) => {
  const lines = [[0, 1, 2],[3, 4, 5],[6, 7, 8],[0, 3, 6],[1, 4, 7],[2, 5, 8],[0, 4, 8],[2, 4, 6]]

  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i]

    if (board[a] && board[a] === board[b] && board[a] === board[c]) {
		if(board[a] === 'X'){
			scoreX++;
		}else{
			scoreO++;
		}
		console.log(board[a] + " " + scoreX + " " + scoreO);
      	return board[a];
    }
  }

  return null
}

const winner = computed(() => CalculateWinner(board.value.flat()));

const MakeMove = (x, y) => {
	if (winner.value) {
		return
	}

	if (board.value[x][y]) return

	board.value[x][y] = player.value

	player.value = player.value === 'X' ? 'O' : 'X'
}

const ResetGame = () => {
	board.value = [
		['', '', ''],
		['', '', ''],
		['', '', '']
	]
	player.value = 'X'
}

</script>

<template>
	<main class="pt-8 text-center">
		<h1 class="mb-8 text-6xl font-bold">TicTacToe Vite App</h1>

		<h3 class="text-3xl mb-4">Player {{ player }}'s turn</h3>

		<h1 class="text-3xl mb-4">Score {{ scoreX }}(X) : {{ scoreO }}(O)</h1>

		<div class="flex flex-col items-center mb-8">
			<div 
				v-for="(row, x) in board" 
				:key="x"
				class="flex">
				<div 
					v-for="(cell, y) in row" 
					:key="y" 
					@click="MakeMove(x, y)" 
					:class="`border border-black w-[140px] h-[140px] hover:bg-gray-100
					flex items-center justify-center material-icons-outlined text-8xl 
					cursor-pointer ${cell === 'X' ? 'text-purple-500' : 'text-yellow-400'}`">
					{{ cell === 'X' ? 'close' : cell === 'O' ? 'circle' : '' }}
				</div>
			</div>
		</div>

		<div class="text-center flex flex-col gap-[20px] items-center">
      		<button @click="ResetGame" class="text-gray-100 text-[20px] px-4 py-2 bg-purple-500 rounded uppercase font-bold hover:bg-purple-700 duration-300 h-[50px] w-[150px]">Reset</button>
			<h2 v-if="winner" class="text-5xl font-bold mb-8">Player '{{ winner }}' wins!</h2>
		</div>
	</main>
</template>

<style>
body {
	@apply text-black;
}
</style>