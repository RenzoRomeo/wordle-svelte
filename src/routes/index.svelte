<script lang="ts">
	const words: Array<Array<string>> = new Array(6).fill(new Array(5).fill(''));

	let playing = true;
	let turn: number = 0;
	const answer: string = 'WRONG';

	const handleKeyDown = (e: KeyboardEvent) => {
		if (!playing) return;

		switch (e.key) {
			case 'Enter': {
				if (words[turn].some((letter) => letter === '')) return;

				if (words[turn].join('') === answer) playing = false;

				turn++;
				return;
			}

			case 'Backspace': {
				let firstEmptyIndex = words[turn].findIndex((letter: string) => letter === '');

				if (firstEmptyIndex === -1) {
					firstEmptyIndex = words[turn].length;
				}

				words[turn][firstEmptyIndex - 1] = '';
				return;
			}

			default: {
				if (e.key.length === 1 && e.key.match(/[a-z]/i)) {
					const firstEmptyIndex = words[turn].findIndex((letter: string) => letter === '');

					const draft = [...words[turn]];
					draft[firstEmptyIndex] = e.key.toUpperCase();

					words[turn] = draft;
				}
				return;
			}
		}
	};
</script>

<svelte:window on:keydown={handleKeyDown} />

<div class="layout">
	<main class="board">
		{#each words as word, wordIndex}
			<section class="word">
				{#each word as letter, letterIndex}
					<article
						class="letter"
						class:correct={letter && wordIndex < turn && letter === answer[letterIndex]}
						class:present={letter &&
							wordIndex < turn &&
							letter !== answer[letterIndex] &&
							answer.includes(letter)}
					>
						{letter}
					</article>
				{/each}
			</section>
		{/each}

		<div class="game-state {playing ? 'playing' : 'not-playing'}">
			{playing ? 'Guess the Word' : 'Game Over'}
		</div>
	</main>
</div>

<style>
	:global(*) {
		box-sizing: border-box;
		margin: 0;
		padding: 0;
		font-family: sans-serif;
	}

	.layout {
		height: 100vh;
		background: linear-gradient(to right, #0f0c29, #302b63, #24243e);
		display: flex;
		justify-content: center;
	}

	.board {
		padding-top: 48px;
		display: grid;
		place-content: center;
		gap: 12px;
	}

	.word {
		display: flex;
		gap: 12px;
	}

	.letter {
		color: white;
		width: 60px;
		height: 60px;
		border: 1px solid white;
		display: flex;
		align-items: center;
		justify-content: center;
		font-weight: bold;
		font-size: 32px;
		text-transform: uppercase;
	}

	.present {
		background-color: goldenrod;
		color: black;
	}

	.correct {
		background-color: springgreen;
		color: black;
	}

	.game-state {
		display: flex;
		justify-content: center;
		font-size: 3rem;
    border-radius: 10px;
    padding: 5px;
	}

	.playing {
		background-color: green;
	}

	.not-playing {
		background-color: red;
	}
</style>
