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
					<div class="input-field col s6 offset-s3">
						<table class="bordered responsive-table highlight centered">
							<thead>
								<tr>
									<th>Word</th>
									<th>Count</th>
								</tr>
							</thead>
							<tbody>
								<tr>
									<td>I</td>
									<td>2</td>
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
			errorMessage: ''
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
			axios.post('http://localhost:3002/words', {count: this.userCount})
				.then(response => {
					console.log(response);
				})
				.catch(error => {
					console.log(error);
				});
		}
	}
}
</script>
