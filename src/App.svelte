<script>
    import Question from './Question.svelte';
    import Answer from './Answer.svelte';

    let BASE_URL = 'https://opentdb.com/api.php?';
    // let URL = `${BASE_URL}amount=10&category=${category}&difficulty=${difficulty}&type=multiple`;
    let triviaURL = `${BASE_URL}amount=3&category=9&difficulty=easy&type=multiple`;
    let currentQuestion = 0;
    let QUESTION_INDEX = 0;
    let ANSWER_INDEX = 1;
    let trivia = [];
    let quizQuestions = [];
    let answers = [];
    let correct = [];
    let quiz = [];

    export const checkAnswer = (answerValue) => {
        console.log(answerValue);
    };

    // Borrowed from stack overflow
    // NOTE TO SELF: read about what it does to solidify understanding
    const mixUpAnswers = (a, b) => {
        return 0.5 - Math.random();
    };

    const getTrivia = async (url) => {
        let response = await fetch(url);
        trivia = await response.json();
        handleTrivia(trivia);
    };

    const handleTrivia = (data) => {
        correct = data.results.map((result) => {
            return [result.correct_answer];
        });

        quizQuestions = data.results.map((result, index) => {
            return (index = {
                triviaQuestion: result.question,
                triviaAnswers: [
                    result.correct_answer,
                    ...result.incorrect_answers
                ]
            });
        });

        answers = data.results.map((result) => {
            return [result.correct_answer, ...result.incorrect_answers];
        });
        correct = correct.flat();
        console.log(correct[currentQuestion], ' bleh');
    };

    //const checkAnswer = () => {
    //		if (quizQuestions[currentQuestion])
    //}
    getTrivia(triviaURL);

    // $: [currentQuestion] = quizQuestions;
    $: score = '';
</script>

<main>
    {#if quizQuestions[currentQuestion]}
        <!-- 		{quizQuestions[currentQuestion].triviaQuestion} -->

        <Question question={quizQuestions[currentQuestion].triviaQuestion} />
    {/if}
    <p>
        {correct[currentQuestion]}
        {#if checkAnswer(answers) === correct[currentQuestion]}
            {score++}
        {/if}
    </p>
    <!--	{#if currentQuestion}
		<p>
			{currentQuestion.triviaQuestion}
			howdy
	</p>
	{/if} -->
    <button
        on:click={() => {
            currentQuestion++;
        }}>Next Question</button
    >
    <div class="answers">
        {#if quizQuestions[currentQuestion]}
            <Answer
                quizAnswer={quizQuestions[currentQuestion].triviaAnswers}
                checkedAnswer
            />

            <!-- 			{quizQuestions[currentQuestion].triviaAnswers}; -->
        {/if}
        <!--    					{#if quizQuestions[currentQuestion]}
						{#each quizQuestions as answer}
						{@html answer.triviaAnswers}
             <Answer quizAnswer={answer.triviaAnswers}/> 
						{/each}
						{/if} -->
    </div>
</main>
