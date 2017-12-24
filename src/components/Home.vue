<template>
	<div class="container">
		<div class="row">
		<h2>Terribly Tiny Tales</h2>
			<form @submit.prevent="submitUserData" class="col s12">
				<div class="row">
					<div class="input-field col s6 offset-s3">
						<input @focus="clearError" v-model="userCount" placeholder="Enter the number count" id="count" type="number" min="1">
						<p v-if="isError">{{errorMessage}}</p>
					</div>
					<div class="input-field col s6 offset-s3">
						<a @click="submitUserData" class="waves-effect waves-light btn">Submit</a>
					</div>
					<div v-if="isDataAvailable" class="input-field col s6 offset-s3">
						<table class="bordered responsive-table highlight centered">
							<thead>
								<tr>
									<th>Word</th>
									<th>Count</th>
								</tr>
							</thead>
							<tbody>
								<tr v-for="(item, index) in wordsData" :key="index">
									<td>{{item[0]}}</td>
									<td>{{item[1]}}</td>
								</tr>
							</tbody>
						</table>
					</div>
				</div>
			</form>
		</div>
	</div>
</template>

<script>
import axios from 'axios';
export default {
	name: 'HelloWorld',
	data () {
		return {
			msg: 'Welcome to Your Vue.js App',
			isDataAvailable: false,
			userCount: 1,
			isError: false,
			errorMessage: '',
			wordsData : []
		}
	},
	methods: {
		clearError(){
			this.isError = false;
			this.errorMessage = '';
		},
		submitUserData(){
			if(this.userCount < 1){
				this.isError = true;
				this.errorMessage = 'Please input a number greater than 0';
				return
			}
			axios.post('https://arcane-wildwood-86545.herokuapp.com/words', {count: this.userCount})
				.then(response => {
					let { data } = response.data;
					this.wordsData = data;
					this.isDataAvailable = true;
				})
				.catch(error => {
					this.isDataAvailable = false;
					this.isError = true;
					this.errorMessage = 'Oops! Looks like something went wrong!';
					console.log(error);
				});
		}
	}
}
</script>
