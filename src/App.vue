<template>
   <div id="app">
		<div class="column is-half is-offset-one-quarter">
			<h1 class="is-size-1">Pokedex</h1>
			<div class="search">
				<input type="text" class="input" v-model="searchValue">
				<button class="button is-primary" @click="search">SEARCH</button>
			</div>
			<div v-for="(pokemon) in filtered" :key="pokemon.name">
				<Pokemon :name="pokemon.name" :url="pokemon.url" />
			</div>
		</div>
   </div>
</template>

<script>
import api from './services/api';
import Pokemon from './components/Pokemon';


export default {
	name: 'App',
	data: function() 
	{
		return {
			pokemons: [],
			filtered: [],
			searchValue: ''
		}
	},

   created: function() {
		api.get('/pokemon', 
		{
			query: {
				limit: 151,
				offset: 0
			}
		})
		.then(data => {
			this.pokemons = data.data.results;
			this.filtered = data.data.results;
		});
	},

	methods: {
		search: function() {
			this.filtered = this.pokemons;
			
			if (!this.searchValue) {
				this.filtered = this.pokemons;
			}
			else {
				this.filtered = this.pokemons.filter(pokemon => {
					return pokemon.name === this.searchValue;
				});
			}
		}
	},

	components: {
		Pokemon
	}
}

</script>

<style>
	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
		margin-top: 60px;
	}

	.search {
		display: flex;
		margin-bottom: 20px;
	}

	.search button {
		margin-left: 5px;
	}

</style>
