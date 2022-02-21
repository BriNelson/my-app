<script>
	
let fetchPromise = fetchTrivia();
let catPromise = fetchCategories();



async function fetchCategories(){
    const catResponse = await fetch("https://opentdb.com/api_category.php");
    console.log(catResponse);
    const catRes = await catResponse.json();
    console.log(catRes.name);
    return catRes;
  }

  

	


    async function fetchTrivia(){
    const response = await fetch("https://opentdb.com/api.php?amount=1");
    console.log(response);
    const res = await response.json();
    console.log(res.results[0].category);
    return res;
  }

  function handleClick(){
	  fetchPromise = fetchTrivia();
	 catPromise = fetchCategories();
  }
</script>



<button on:click={handleClick}> get Trivia question </button>

{#await catPromise}
loading
{:then data}
{data.trivia_categories[0].name}
{:catch error}
  <p>error</p>
{/await}

<select name="difficulty" id="difDropdown">
  <option value="volvo">Easy</option>
  <option value="saab">Medium</option>
  <option value="mercedes">Hard</option>
</select>

<select name="difficulty" id="difDropdown">
  <option value="volvo">Any Category</option>
  <option value="saab">Medium</option>
  <option value="mercedes">Hard</option>
</select>

{#await fetchPromise}
  <p>whatever this is</p>
{:then data}
  <p>{data.results[0].category}</p>
  <p>{data.results[0].question}</p>

  <h3>Answers</h3>

  <form>
    <input type="radio" id="html" name="fav_language" value="HTML" />
    <label for="html">{data.results[0].correct_answer}</label><br />
    <input type="radio" id="html" name="fav_language" value="HTML" />
    <label for="html">{data.results[0].incorrect_answers[0]}</label><br />
    <input type="radio" id="html" name="fav_language" value="HTML" />
    <label for="html">{data.results[0].incorrect_answers[1]}</label><br />
    <input type="radio" id="html" name="fav_language" value="HTML" />
    <label for="html">{data.results[0].incorrect_answers[2]}</label><br />
  </form>




{:catch error}
  <p>error</p>
{/await}
<!-- * As a user, I want to be asked trivia questions -->
<!-- * As a user, I want to be able to answer trivia questions -->
<!-- * As a user, I want to know how many questions I have answered correctly or wrong. -->
<!-- * As a user, I want to be able to select a difficulty level. -->
<!-- * As a user I want to be able to select a category for the game. -->
<!-- * App must be hosted at a live URL -->
<!-- https://opentdb.com/api_config.php -->
