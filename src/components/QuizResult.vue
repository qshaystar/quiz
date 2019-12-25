<template>
	<div class="result-box-container">

		<b-jumbotron>
			<template v-slot:lead>
				<p>Your Score: {{ numCorrect }}</p>
				<p class="result" v-text="resultCard(numCorrect)"><b> </b></p>
			</template>		
			<b-button variant="info"
				@click="refresh"
			>
				Try Again?
			</b-button>
		</b-jumbotron>
		
	</div>
</template>

<script>
	export default {
		props: ['numCorrect'],

		methods: {
			resultCard(score) {
				if(score >= 8) {
					return "Nailed It!"
				}else if(score >= 4) {
					return "Perfet!"
				}else {
					return "lol Bad Luck?!"
				}
			},

			refresh() {
				location.reload()
			}
		}
		
	}
</script>

<style scoped>
	.result-box-container {
		margin-top: 60px;
	}
	
	p {		
		font-size: 25px;
	}

	.result {
		display: block;
		font-size: 45px;
	}

</style>