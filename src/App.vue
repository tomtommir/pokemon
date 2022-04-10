<template>
	<div id="app" class="app">
		<img class="logo-pokemon" alt="logo" src="/images/pokemon.png">
		<div class="search-container">
			<i 
				class="ri-search-line search-button"
				@click="searchPokemon()"></i>
			<input 
				type="text" 
				class="search-bar" 
				v-model="search" 
				placeholder="Search"
				v-on:keyup.enter="searchPokemon()"/>
			<i 
				class="ri-close-line search-reset" 
				@click="searchReset()"></i>
		</div>
		<div class="container-list">
			<PokemonList 
				ref="pokemonList"
				:search="searchList"
			/>
			<PokemonInfo
				:name="namePokemon"
				:url="urlPokemon"
			/>
		</div>
	</div>
</template>

<script>
import PokemonInfo from "./components/PokemonInfo"
import PokemonList from "./components/PokemonList"

export default {
	name: "App",
	components: {
		PokemonInfo,
		PokemonList,
	},
	data() {
		return {
			namePokemon: "Pikachu",
			urlPokemon: "https://pokeapi.co/api/v2/pokemon/pikachu",
			search: "",
			searchList: ""
		}
	},
	methods:{
		majData(name,url){
			this.namePokemon = name;
			this.urlPokemon = url;
			this.searchList = this.search
		},
		searchPokemon(){
			if(this.search == ""){
				return false
			}else{
				this.majData(this.search,"https://pokeapi.co/api/v2/pokemon/"+this.search)
			}
		},
		searchReset(){
			//On vide le champ recherche
			this.search = ""
			//On initialise les champs Pour le component PokemonInfo
			this.namePokemon = "Pikachu";
			this.urlPokemon = "https://pokeapi.co/api/v2/pokemon/pikachu";
			//On initialise la liste en appelant la fonction de PokemonList
			this.$refs.pokemonList.getPokemonListFirst()
		}
	},
	mounted(){

	}
}
</script>

<style lang="scss">
	#app{
		text-align: center;
		*{
			box-sizing: border-box;
		}
	}
	.app {
		font-family: "Avenir", Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
		margin: 50px 20px;
	}
	.logo-pokemon{
		margin-bottom: 50px;
	}
	.nav{
		display: flex;
		flex-direction: row;
		column-gap: 50px;
		justify-content: center;
	}
	.search-container{
		width: 400px;
		margin: 0 auto;
		position: relative;
		.search-button{
			cursor: pointer;
			position: absolute;
			left: 15px;
			top: 50%;
			transform: translateY(-50%);
		}
		.search-reset{
			cursor: pointer;
			position: absolute;
			right: 15px;
			top: 50%;
			transform: translateY(-50%);
		}
		.search-bar{
			width: 100%;
			padding: 10px 40px;
			border: none;
			border-radius: 20px;
			border: 2px solid #3264af;
			transition: all 400ms ease-in;
			&:focus-visible{
				outline: none;
			}
			&:focus,&:active{
				outline: none;
				border: 2px solid #17285e;
			}
		}
	}
	.container-list{
		width: 100%;
        margin: 50px auto;
		display: flex;
		column-gap: 20px;
	}
	
	@media screen and (max-width: 1100px) {
		.app{
			margin-top: 20px;
		}
		.nav{
			column-gap: 20px;
		}
	}

	@media screen and (max-width: 500px) {
		.search-container{
			width: 100%;
		}

	}

</style>
