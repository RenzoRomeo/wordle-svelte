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
</main>

<style>
	:global(*) {
		box-sizing: border-box;
		margin: 0;
		padding: 0;
		font-family: sans-serif;
	}

	.board {
		margin-top: 48px;
		display: grid;
		place-content: center;
		gap: 12px;
	}

	.word {
		display: flex;
		gap: 12px;
	}

	.letter {
		width: 48px;
		height: 48px;
		border: 1px solid gray;
		display: flex;
		align-items: center;
		justify-content: center;
		font-weight: bold;
		font-size: 32px;
		text-transform: uppercase;
	}

	.present {
		background-color: goldenrod;
	}

	.correct {
		background-color: springgreen;
	}
</style>
