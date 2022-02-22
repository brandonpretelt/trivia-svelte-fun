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
    let test = [];
    let testAns = [];
    let new_triviaQuestion;
    let new_triviaAnswer;

    // Borrowed from stack overflow
    // NOTE TO SELF: read about what it does to solidify understanding
    const mixUpAnswers = (a, b) => {
        return 0.5 - Math.random();
    };

    const getTrivia = async (url) => {
        let response = await fetch(url);
        trivia = await response.json();
        // console.log(handleTrivia(trivia));
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

        console.log(quizQuestions, ' test');

        answers = data.results.map((result) => {
            return [result.correct_answer, ...result.incorrect_answers];
        });
        // answers = answers.sort(mixUpAnswers);
        correct = correct.flat();

        //         console.log(quizQuestions[currentQuestion].triviaQuestion);
        console.log(quizQuestions[0]);
    };

    getTrivia(triviaURL);

    // $: [currentQuestion] = quizQuestions;

    // 	console.log(quizQuestions[currentQuestion], 'test')
</script>

<main>
    {#if quizQuestions[currentQuestion]}
        <!-- 		{quizQuestions[currentQuestion].triviaQuestion} -->

        <Question question={quizQuestions[currentQuestion].triviaQuestion} />
    {/if}

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
            <Answer quizAnswer={quizQuestions[currentQuestion].triviaAnswers} />

            {quizQuestions[currentQuestion].triviaAnswers};
        {/if}
        <!--    					{#if quizQuestions[currentQuestion]}
						{#each quizQuestions as answer}
						{@html answer.triviaAnswers}
             <Answer quizAnswer={answer.triviaAnswers}/> 
						{/each}
						{/if} -->

        la la la
    </div>
</main>
