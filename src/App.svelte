<script>
    let difficulty, category, selected;
    let BASE_URL = 'https://opentdb.com/api.php?';

    const radioOpts = [
        { value: 9, textContent: 'General Knowledge' },
        { value: 11, textContent: 'Film' },
        { value: 14, textContent: 'TV' },
        { value: 15, textContent: 'Video Games' },
        { value: 31, textContent: 'Anime' }
    ];

    const selectOpts = [
        { textContent: 'Easy' },
        { textContent: 'Medium' },
        { textContent: 'Hard' }
    ];

    let handledCategory = 'test';
    // let question = '';
    let questions = [];
    let count = 0;
    let answers = [];

    const sortFunc = (a, b) => {
        return 0.5 - Math.random();
    };

    const getTriviaData = async (url) => {
        let response = await fetch(url);
        let data = await response.json();

        handleTriviaData(data);
    };

    const handleTriviaData = (data) => {
        handledCategory = data.results[count].category;
        // question = data.results[count].question;

        questions = data.results;
        answers = data.results.map((result) => {
            answers = [result.correct_answer, ...result.incorrect_answers];
        });
        /*         answers = [
            data.results[0].correct_answer,
            ...data.results[0].incorrect_answers
        ]; */
        answers = answers.flat().sort(sortFunc);
        let the_correct_answer = data.results[count].correct;

        // console.log(answers.flat());
    };

    const checkAnswer = () => {
        const paragraphAnswers = document.querySelectorAll('.answers > p');
        paragraphAnswers.forEach((answer) => {
            console.log(answer.textContent);
        });
    };

    const handleSubmit = () => {
        const radios = document.querySelectorAll('input[type="radio"]');
        radios.forEach((radio) => {
            if (radio.checked) {
                category = radio.value;
            }
        });

        if (!difficulty) {
            difficulty = 'easy';
        } else if (selected && difficulty) {
            difficulty = selected.textContent.toLowerCase();
        }
        let URL = `${BASE_URL}amount=10&category=${category}&difficulty=${difficulty}&type=multiple`;
        console.log(URL, ' after');
        getTriviaData(URL);
    };
</script>

<main>
    <h3>le sigh</h3>
    <form on:submit|preventDefault={handleSubmit}>
        <div>
            <h3>Please Select a Category</h3>
            {#each radioOpts as { value, textContent }, index}
                <label>
                    <input
                        type="radio"
                        {value}
                        name="category"
                        bind:group={category}
                    />

                    <span>{textContent}</span>
                </label>
            {/each}
        </div>
        <div>
            <label for="difficulty">Please Select a Difficulty</label>
            <select bind:value={selected} name="difficulty" id="difficulty">
                {#each selectOpts as opts}
                    <option value={opts}>
                        {opts.textContent}
                    </option>
                {/each}
                <!--                 <option bind={difficulty}>Easy</option>
                <option bind={difficulty}>Medium</option>
                <option bind={difficulty}>Hard</option> -->
            </select>
        </div>
        <button>Start Quiz</button>
    </form>
    <div class="output">
        <h1>{handledCategory}</h1>
        <!--        <div class="question">
            {@html question}
        </div> -->
        <div class="questions">
            {#each questions as question}
                <p>{@html question.question}</p>
            {/each}
        </div>
        <div class="answers">
            {#each answers as answer}
                <p>{@html answer.answers}</p>
            {/each}
        </div>
        <button on:click={checkAnswer}>Check Answer</button>
    </div>
</main>

<style>
    main {
        margin: 0;
    }
    .answers > p {
        background-color: gray;
        padding: 1em;
        color: #fff;
        width: 10vw;
        cursor: pointer;
    }
</style>
