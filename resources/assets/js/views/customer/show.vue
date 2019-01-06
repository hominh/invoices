<template>
	<div class="panel panel-default">
		<div class="panel-heading">
			<span class="panel-title">
				{{model.company}} / {{model.name}}
			</span>
			<div>
				<router-link :to="'/customer/'+model.id+'/edit'" class="btn btn-primary btn-sm">Edit</router-link>
				<button class="btn btn-danger btn-sm" @click="remove">Delete</button>
			</div>
		</div>
		<div class="panel-body">
			<div class="row">
				<div class="col-sm-4">
					<label>Company</label>
					<p>{{model.company}}</p>
					<label>Name</label>
					<p>{{model.name}}</p>
				</div>
				<div class="col-sm-4">
					<label>Email</label>
					<p>{{model.email}}</p>
					<label>Phone</label>
					<p>{{model.phone}}</p>
				</div>
				<div class="col-sm-4">
					<label>Created at</label>
					<p>{{model.created_at}}</p>
					<label>Address</label>
					<p>{{model.address}}</p>
				</div>
			</div>
		</div>
	</div>
</template>
<script>
	import Vue from 'vue'
	import axios from 'axios'
	export default {
		name: 'CategoryShow',
		data() {
			return {
				model: {},
				resource: 'customer',
				redirect: '/'
			}
		},
		beforeMount() {
			this.fetchData()
		},
		watch: {
			'$route': 'fetchData'
		},
		methods: {
			fetchData() {
				console.log('fetchData')
				var vm = this
				axios.get(`/api/${this.resource}/${this.$route.params.id}`)
					.then(function(response){
						Vue.set(vm.$data, 'model', response.data.model)
					}).catch(function(error){
						console.log(error)
					})
			},
			remove() {
				var vm = this
				axios.delete(`api/${this.resource}/${this.$route.params.id}`)
					.then(function(response) {
						if(response.data.deleted) {
							vm.$router.push(vm.redirect)
						}
					})
					.catch(function(error) {
						console.log(error)
					})
			}
		}
	}
</script>