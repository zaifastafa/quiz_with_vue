<template>
    <div class="question-box-container">
        <b-jumbotron>

            <template slot="lead">
                {{ currentQuestion.question }}
            </template>

            <hr class="my-4">

            <b-list-group>
                <b-list-group-item
                        v-for="(answer, index) in answers"
                        :key="index"
                        @click="selectAnswer(index)"
                        :class="[answerClass(index)]"
                >
                    {{ answer }}
                </b-list-group-item>
            </b-list-group>


            <b-button
                    variant="primary"
                    @click="submitAnswer"
                    :disabled="selectedIndex === null || answered"
            >
                Submit
            </b-button>
            <b-button variant="success" @click="next">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
    import _ from 'lodash'

    export default {
        name: "QuestionBox",
        props: {
            currentQuestion: Object,
            next: Function,
            increment: Function
        },
        data() {
            return {
                selectedIndex: null,
                correctIndex: null,
                shuffledAnswers: [],
                answered: false
            }
        },
        computed: {
            answers() {
                let answers = [...this.currentQuestion.incorrect_answers];
                answers.push(this.currentQuestion.correct_answer);

                return answers;
            }
        },
        watch: {
            currentQuestion: {
                immediate: true,
                handler() {
                    this.selectedIndex = null;
                    this.shuffleAnswers();
                    this.answered = false
                }
            }
        },
        methods: {
            selectAnswer(index) {
                if (!this.answered)
                    this.selectedIndex = index;
            },
            shuffleAnswers() {
                let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
                this.shuffledAnswers = _.shuffle(answers);
                this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
            },
            submitAnswer() {
                let isCorrect = false;

                if (this.selectedIndex === this.correctIndex) {
                    isCorrect = true
                }

                this.answered = true;

                this.increment(isCorrect);
            },
            answerClass(index) {
                let answerClass = '';

                answerClass = !this.answered && this.selectedIndex === index ? 'selected' :
                    this.answered && this.correctIndex === index ? 'correct' :
                        this.answered && this.selectedIndex === index && this.correctIndex !== index ? 'incorrect' : '';

                return answerClass;
            }
        }
    }
</script>

<style scoped>
    .list-group {
        margin-bottom: 15px;
    }

    .list-group-item:hover {
        cursor: pointer;
        background: #EEE;

    }

    .btn {
        margin: 0 5px
    }

    .selected {
        background-color: lightskyblue;
    }

    .correct {
        background-color: lightgreen;
    }

    .incorrect {
        background-color: lightcoral;
    }
</style>
