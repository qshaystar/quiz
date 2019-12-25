
<template>
	
	<div class="question-box-container">
		<b-jumbotron>
			<template v-slot:lead>
				{{ currentQuestion.question | replaceHtmlCode }}
			</template>		
			<hr class="my-4">
			<b-list-group>

			  <b-list-group-item
				v-for="(answer, index) in shuffledAnswers" 
				:key="index" 
				@click="selectedAnswer(index)"
				:class= "answerClass(index)"
			  >

			  {{ answer | replaceHtmlCode }}

			  </b-list-group-item>

			</b-list-group>
			
			<b-button variant="primary" 
			@click="submitAnswer"
			:disabled="selectedIndex === null || answered"
			>
			Submit
			</b-button>
			<b-button variant="success" 
			@click="next"
			:disabled="!answered"
			>
			Next
			</b-button>
		</b-jumbotron>
	</div>	

</template>

<script>
	import _ from 'lodash'

	export default {
		props: {
			currentQuestion: Object,
			next: Function,
			increment: Function
		},
		data: function() {
			return {
				selectedIndex: null,
				correctIndex: null,
				shuffledAnswers: [],
				answered: false,
			}
		},
		// 每次currentQuestion array改變時，更換answer順序
		watch: {
			currentQuestion: {
				// 讓監聽值在初始和值被改變時觸發 callback handler，若未用click瞬間shuffle但顯示時改回原本順序
				immediate: true,
				handler() {
					this.selectedIndex = null
					this.answered = false
					this.shuffleAnswers()
				}			
			}
		},
		computed: {
			answers() {
				// 若未加this結果為undefined記得加
				// [...value.value]成為一個陣列，而非[[value], [value]]
				let answers = [...this.currentQuestion.incorrect_answers]
				answers.push(this.currentQuestion.correct_answer)
				return answers
			}
		},
		methods: {
			selectedAnswer(index) {
				this.selectedIndex = index
			},
			submitAnswer() {
				let isCorrect = false

				if(this.selectedIndex ===  this.correctIndex ) {
					isCorrect = true
				}

				this.answered = true

				this.increment(isCorrect)
			},
			shuffleAnswers() {
				let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
				this.shuffledAnswers = _.shuffle(answers)
				this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
			},
			answerClass(index) {

				let answerClass = ''

				// this.selectedIndex === index > 被選到的index === for循環吐出來的index

				if(!this.answered && this.selectedIndex === index) {
					answerClass = 'selected'
				}else if(this.answered && this.correctIndex === index ) {
					answerClass = 'correct'

				// 回答了 並且被選到的index與for循環的index相同(就是指被選到的index) 並且與correctIndex不同 

				}else if(this.answered && this.selectedIndex === index && this.correctIndex !== index ) {
					answerClass = 'incorrect'
				}

				return answerClass
			}
		},

		filters: {
			replaceHtmlCode(value) {
				return value
				 .replace(/&amp;/g, '&')
		         .replace(/&lt;/g, '<')
		         .replace(/&gt;/g, '>')
		         .replace(/&quot;/g, '"')
		         .replace(/&#039;/g, "'")
		         .replace(/&shy;/g, '-')
		         .replace(/&lrm;/g, '→')
			}
		}
	}
</script>

<style scoped>
	.list-group {
		margin-bottom: 15px;		
	}

	.list-group-item {
		transition: background-color .3s;
	}

	.list-group-item:hover {
		background-color: #eee;
		cursor: pointer;
	}

	.btn {
		margin: 0 10px;
	}

	.list-group-item.selected {
		background-color: lightblue; 
	}

	.list-group-item.correct {
		background-color: lightgreen;
	}

	.list-group-item.incorrect {
		background-color: red;
	}
</style>
