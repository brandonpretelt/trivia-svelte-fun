<script>
    export let quizAnswer = [];
    export let answers = '';
    export let checkedAnswer = '';
    $: answerValue = '';
    $: checkedAnswer = answerValue;
    $: console.log(answerValue);
</script>

<div>
    {#each quizAnswer as answer}
        <input
            type="radio"
            bind:group={answers}
            name="answers"
            value={answer}
            on:change={() => {
                answerValue = answer;
            }}
        />{answer}<br />
    {/each}

    {answers}

    <!--     <input type="radio" name="answers" bind:group={quizAnswer} value={quizAnswer}>{quizAnswer} -->
</div>

<style>
    div {
        background-color: var(--gray-3);
        margin-bottom: var(--size-2);
        padding: 0.64em;
    }
    /*     p {
        font-size: var(--size-4);
    } */
</style>
