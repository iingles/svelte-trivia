<script>
    import Question from './Question.svelte'
	import Answer from './Answer.svelte'

	export let question
	export let correctAnswer
	export let incorrectAnswers

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
</style>

<div class="box">
	<Question question={question} />

	{ #each answers as a }
		<Answer answer={a.answer} type={a.type} />
	{ /each }

</div>