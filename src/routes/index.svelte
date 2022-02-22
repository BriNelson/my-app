<script>
 let userAnswer = []; 
  let fetchPromise = fetchTrivia();
  let catPromise = fetchCategories();
  let show = false;
  let correctUserAnswers = [];
  let incorrectUserAnswers = [];

  async function fetchCategories() {
    const catResponse = await fetch("https://opentdb.com/api_category.php");
    console.log(catResponse);
    const catRes = await catResponse.json();
    // console.log(catRes.name);
    return catRes;
  }

  async function fetchTrivia() {
    const response = await fetch(
      "https://opentdb.com/api.php?amount=1&category=" +
        selectedCategory +
        "&difficulty=" +
        selectedDifficulty
    );
    
    const res = await response.json();
// let incorrectArray = res.results[0].incorrect_answers
// let correctAnswer = res.results[0].correct_answer
//   let answerArray = [...incorrectArray, correctAnswer]
//   console.log(answerArray)
	
	return res;
    
  }



  let selectedCategory;
  let selectedDifficulty;
  catPromise = fetchCategories();
  function handleClick() {
    fetchPromise = fetchTrivia();
	console.log(fetchPromise.results)
  
  }
    
    
  function handleAnswerClick() {
show = true;
    console.log()
  }


</script>
{show}
<form on:submit|preventDefault={handleClick}>
  {#await catPromise}
    loading
  {:then data}
    <!-- {data.trivia_categories[0].name} -->
    <select bind:value={selectedCategory} id="difDropdown">
      <option value="">Any Category</option>
      {#each data.trivia_categories as datas}
        <option value={datas.id}>{datas.name}</option>
      {/each}
    </select>
  {:catch error}
    <p>clck button to get trivia question</p>
  {/await}

  <select bind:value={selectedDifficulty} id="difDropdown">
    <option value="">Any Difficulty</option>
    <option value="easy">Easy</option>
    <option value="medium">Medium</option>
    <option value="hard">Hard</option>
  </select>
  <button> Get Trivia Question </button>
</form>
{#await fetchPromise}
  <p>loading question</p>
{:then data}

  <p>{data.results[0].category}</p>
  <p>{data.results[0].question}</p>

  <h3>Answers</h3>
  {#if userAnswer == data.results[0].correct_answer}
  <p>Correct answer</p>
  {/if}
 <!-- on submit I want to find out if my answer is clicked -->
 <!-- I also want to push to my correct array if correct and incorect if not -->
  <form on:submit|preventDefault={handleAnswerClick}> 
{#each [...data.results[0].incorrect_answers, data.results[0].correct_answer] as datas} <!-- ugly but works -->
<input type="radio" bind:group = {userAnswer} value={datas} />
<label for="html">{datas}</label><br />
{/each}

<button>Submit Answer</button>
</form>
  
{:catch error}
  <p>clck button to get trivia question</p>
{/await}
<!-- * As a user, I want to be asked trivia questions -->
<!-- * As a user, I want to be able to answer trivia questions -->
<!-- * As a user, I want to know how many questions I have answered correctly or wrong. -->
<!-- * As a user, I want to be able to select a difficulty level. -->
<!-- * As a user I want to be able to select a category for the game. -->
<!-- * App must be hosted at a live URL -->
<!-- https://opentdb.com/api_config.php -->

<style>
  form, p, h3 {
    font-family: -apple-system,system-ui,"Segoe UI",Helvetica,Arial,sans-serif,"Apple Color Emoji","Segoe UI Emoji";
  }
  select {
    border: 1px solid rgba(27, 31, 35, .15);
  border-radius: 6px;
  box-shadow: rgba(27, 31, 35, .1) 0 1px 0;
  box-sizing: border-box;
  cursor: pointer;
  padding: 6px 16px;
  font-family: -apple-system,system-ui,"Segoe UI",Helvetica,Arial,sans-serif,"Apple Color Emoji","Segoe UI Emoji";
  font-size: 14px;
  line-height: 20px;
  line-height: 20px;
  }
    button {
  appearance: none;
  background-color: #2ea44f;
  border: 1px solid rgba(27, 31, 35, .15);
  border-radius: 6px;
  box-shadow: rgba(27, 31, 35, .1) 0 1px 0;
  box-sizing: border-box;
  color: #fff;
  cursor: pointer;
  display: inline-block;
  font-family: -apple-system,system-ui,"Segoe UI",Helvetica,Arial,sans-serif,"Apple Color Emoji","Segoe UI Emoji";
  font-size: 14px;
  font-weight: 600;
  line-height: 20px;
  padding: 6px 16px;
  position: relative;
  text-align: center;
  text-decoration: none;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  vertical-align: middle;
  white-space: nowrap;
}

button:focus:not(:focus-visible):not(.focus-visible) {
  box-shadow: none;
  outline: none;
}

button:hover {
  background-color: #2c974b;
}

button:focus {
  box-shadow: rgba(46, 164, 79, .4) 0 0 0 3px;
  outline: none;
}

button:disabled {
  background-color: #94d3a2;
  border-color: rgba(27, 31, 35, .1);
  color: rgba(255, 255, 255, .8);
  cursor: default;
}

button:active {
  background-color: #298e46;
  box-shadow: rgba(20, 70, 32, .2) 0 1px 0 inset;
}
  

</style>





