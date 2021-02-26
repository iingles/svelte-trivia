<script>
    import { questions, setToken } from './stores.js'


    const levels = [
        {id: 1, text: "Easy"},
        {id: 2, text: "Medium"},
        {id: 3, text: "Hard"}
    ]

    let categories = []
    
    getCategories().then(res => categories = [...res.trivia_categories] )

    async function getCategories() {
        const url = 'https://opentdb.com/api_category.php'

        const response = await fetch(url)

        return await response.json()
    }


    let selectedDifficulty, 
        selectedCategory

    async function getQuestions() {  
        const tokenUrl = `https://opentdb.com/api_token.php?command=request`      
        let token_value
        let getToken
        let token
        
        const theToken = setToken.subscribe(val => {
            token_value = val
        })

        if (token_value === '') {
            getToken = await fetch(tokenUrl)
            token = await getToken.json()

            setToken.update(() => token.token)
        } else {
            token = token_value
        }

        const url = `https://opentdb.com/api.php?amount=10&difficulty=${selectedDifficulty}&category=${selectedCategory}&token=${token}&type=multiple`
        console.log(url)
       
        const response = await fetch(url)

        return await response.json()

    }

    function setQuestions() {
        getQuestions().then(res => {
            if (questions !== undefined || questions !== 'undefined') questions.update(() => res)
        })
    }

</script>

<style>
    .options {        
        width: 100%;
        text-align: center;
        margin-bottom: 1rem;
    }

    .options select {
        width: 100%;
        border: 1px solid #ccc;
        font-size: 12px;
        cursor: pointer;
    }
</style>

<div class="options">	 

    <form on:submit|preventDefault={ setQuestions }>

        <label for="difficultySelect">Difficulty:</label>

        <select name="difficultySelect" bind:value={ selectedDifficulty }>
            { #each levels as level }
                <option value={ level.text.toLowerCase() }>{ level.text }</option>
            { /each }
        </select>

        <label for="categorySelect">Category:</label>

        <select name="categorySelect" bind:value={ selectedCategory }>
            { #each categories as category }
                <option value={ category.id }>{ category.name }</option>
            { /each }
        </select>

        <button type="submit">Get Questions</button>

    </form>
    
</div>