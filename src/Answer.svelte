<script>
    export let answer
    export let type

    function handleAnswer(evt) {
        const el = evt.target

        if(el.classList.contains('i')) {
            el.style.backgroundColor = 'red'
            el.style.color = 'white'
        }

        if(el.classList.contains('c')) {
            el.style.backgroundColor = 'green'
            el.style.color = 'white'
        }
    }
</script>

<style>
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
</style>

<div class={type + ' answer'} on:click={handleAnswer}>
	<h3>{answer}</h3>
</div>