<script>
    import Question from './Question.svelte'

	export let question
	export let correctAnswer
	export let incorrectAnswers

	let current = ''

	function shuffle(arr) {
		// Fisher-Yates shuffle algorithm - https://github.com/coolaj86/knuth-shuffle
		let currentIndex = arr.length, temp, random

		while (0 !== currentIndex) {
			// Pick a remaining element
			random = Math.floor(Math.random() * currentIndex)
			currentIndex -= 1

			// Swap it with the current element
			temp = arr[currentIndex]
			arr[currentIndex] = arr[random]
			arr[random] = temp
		}

		return arr
	}

	const answers = [
		{type: 'c', answer: correctAnswer}
	]

	incorrectAnswers.forEach(el => {
		answers.push({
			type: 'i',
			answer: el
		})
	})

	shuffle(answers)

</script>

<style>
	.box {
		display: flex;
		flex-direction: column;
		justify-content: center;
		width: 100%;

		border-radius: 2px;
		box-shadow: 2px 2px 8px rgba(0,0,0,0.1);
		padding: 1em;
		margin: 0 0 1em 0;
	}

	.answer {
        display: flex;        
        justify-content: space-between;
        align-items: center;
        padding: 1rem;
        margin-bottom: 1rem;
        border: 1px solid #ccc;
        cursor: pointer;
    }

    .answer:hover {
        background: #f1f1f1;
    }

    .answer:active {
        background: #ccc;
    }

    .answer h3 {
        width: 100%;
        text-align: center;
    }

    .selected.i {
        background: red;
        color: #fff;
    }

    .selected.c {
        background: green;
        color: #fff;
    }
</style>

<div class="box">
	<Question question={question} />

	{ #each answers as a }
		<div 
		class:selected="{current === a.answer}"
		on:click="{() => current = a.answer}"
		class={a.type + " " + 'answer'}
		>
			<h3>{a.answer}</h3>
		</div>
	{ /each }

</div>