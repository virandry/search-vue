<template>
  <div class="container my-5 px-0">
    <input class="form-control" type="text" v-model="searchTerm"
        @input="searchBooks()"
        @keyup.enter = 'displayResult'
        @keyup.down = 'cursorDown'
        @keyup.up = 'cursorUp' />
    <ul class="dropdown-menu" v-if="toggleDDL">
      <li v-for="(item,index) in receivedBooks" class="dropdown-item" @click="selectItem(item)" :class="{'active': isActive(index)}">
        <a href="#">{{ item.name }}</a>
      </li>
    </ul>
    <div class="row">
    	<div v-for="item in results" :key="item.id" class="col-md-4 col-sm-6 col-xs-12 p-3">
    		<div  class="card">
			  <div class="card-block">
			    <h4 class="card-title">{{item.author}}</h4>
			    <p class="card-text">
			    	<span v-if="item.series != null">Series: {{item.series}}</span>
			    	<span>Title: {{item.name}}</span>
			    </p>
			  </div>
			</div>
    	</div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
const url = 'http://localhost:1110/books?author=';

export default {
  name: 'Search',
  data () {
    return {
      title: 'Mini Search engine',
      toggleDDL: false,
      searchTerm: '',
      receivedBooks: [],
      results:[],
      iter:null,
    }
  },
  watch: {
		searchTerm: function (val) {
			if(val.length == 0){
				this.results = [];
			}
		}
	},
  methods:{
		searchBooks: function(){
			var obj = this;
			if (obj.searchTerm.length > 0){
				axios.get(url+obj.searchTerm)
				.then(function (response) {
					console.log(response.data);
					var result = response.data;
					if (typeof result.error === 'undefined'){
						obj.receivedBooks = result;
						obj.toggleDDL = true;
					} else {
						obj.toggleDDL = false;
					}

				})
				.catch(function (error) {
					console.log(error);
					obj.toggleDDL = false;
				});
			} else {
				obj.toggleDDL = false;
			}

		},
		displayResult: function(){
			if (this.iter==null){
				this.results = this.receivedBooks;
			} else {
				this.results = [];
				this.results.push(this.receivedBooks[this.iter]);
			}
			this.toggleDDL = false;
			this.iter = null;
		},
		selectItem: function(item){
			this.results = [];
			this.results.push(item);
			this.toggleDDL = false;
			this.iter = null;
		},
		cursorDown: function(){
			console.log(this.iter < this.receivedBooks.length - 1);
			if (this.iter == null){
				this.iter = 0
			} else if(this.iter < this.receivedBooks.length - 1){
				this.iter++;
			}
	        console.log(this.iter);
		},
		cursorUp: function(){
			console.log(this.iter);
			if(this.iter > 0){
				this.iter--;
			}
			console.log(this.iter);
		},
		isActive(index) {
	        return index === this.iter;
	    },
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.dropdown-menu {
z-index: 2;
display: block;
width: 100%;
top: 40px;
}

.dropdown-item.v-selected {
color: #1d1e1f;
text-decoration: none;
background-color: #f7f7f9;
}

.dropdown-item.active a {
color: #fff;
text-decoration: none;
}

.dropdown-item:active {
color: #1d1e1f;
text-decoration: none;
background-color: #f7f7f9;
}

a:visited {
text-decoration: none;
}

a:hover {
text-decoration: none;
}

a:active {
text-decoration: none;
}

.card-text span{
display: block;
}
</style>
