<script>
    export let quizAnswer = [];
</script>

<div>
    {#each quizAnswer as answer}
        <input type="radio" name="answers" value={answer} />{answer}<br />
    {/each}
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
