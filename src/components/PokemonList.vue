<template>
	<section class="pokemon-item">
		<div v-if="loading" class="loading">
            <img src="/images/pokeball.svg" class='loader'>
        </div>
        <div v-else>
			<div v-if="error" class="error">
				Problem while listing pokemon list
			</div>
			<div v-else>
                <ul class="pokemon-list">
                    <li v-for="pokemon in pokemonList" :key="pokemon.name" @click="getInfoPokemon(pokemon.url,pokemon.name)">
                        <div class="image-container">
                            <img class="pokemon-img" :src="pokemon.img" :alt="pokemon.name" />
                        </div>
                        <span>{{ pokemon.name }}</span>
                        <i class="ri-arrow-right-s-line"></i>
                    </li>
                </ul>
                <div v-if="noSearch" class="error">
                    <Paginate
                        v-model="page"
                        :page-count="maxPage"
                        :page-range="3"
                        :margin-pages="2"
                        :click-handler="paginationClick"
                        :prev-text="'Prev'"
                        :next-text="'Next'"
                        :container-class="'pagination'"
                        :page-class="'page-item'"
                    />
                </div>
			</div>
        </div>
	</section>
</template>

<script>
import axios from "axios"
import Paginate from "vuejs-paginate-next"

export default {
	name: "PokemonList",
	props: ["search"],
    components: {
		Paginate
	},
	watch: {
        search: function(search) {
			this.getPokemonListSearch(search)
        }
	},
	data() {
		return {
            maxPage: "",
            pokemonByPage: 10,
			loading: false,
            url: "https://pokeapi.co/api/v2/pokemon/?limit=10",
            urlSearch: "https://pokeapi.co/api/v2/pokemon/?limit=1200",
            pokemonList:[],
            noSearch: true,
		}
	},
	methods: {
        paginationClick(num){
            this.loadingList = true
            let newOffset = (num-1) * this.pokemonByPage
            let newUrl = "https://pokeapi.co/api/v2/pokemon/?offset="+newOffset+"&limit="+this.pokemonByPage
            this.getPokemonList(newUrl)
        },
        getPokemonListImg(url,key){
            axios.get(url)
            .then(response => {
                //On vient retourner l'image du pokemon dans l'objet pokemonList
                this.pokemonList[key].img = response.data.sprites.front_default
                this.pokemonList[key].url = url
            })
            .catch(error => { 
                console.log(error)
            })
        },
		getPokemonList(url){
            const self = this
            axios.get(url)
            .then(response => {
                //On vient mettre la liste à jour
                this.pokemonList = response.data.results

                //on vient mettre à jour l'img pour chaque objet pokemon
                //en appelant via l'url de chaque pokemon
                this.pokemonList.map(function(value, key) {
                    self.getPokemonListImg(value.url,key)
                })
            })
            .catch(error => { 
                console.log(error)
            })
            .finally(() => (this.loadingList = false))
		},
        getPokemonListFirst(){
            const self = this
			this.loading = true
			this.noSearch = true

			setTimeout(()=>{
				axios.get(self.url)
				.then(response => {
                    //On met à jour la pagination avec le nombre de page max
                    this.maxPage = Math.ceil(response.data.count / this.pokemonByPage) - 1 
                    
                    //On vient mettre la liste à jour
					this.pokemonList = response.data.results

                    //on vient mettre à jour l'img pour chaque objet pokemon
                    //en appelant via l'url de chaque pokemon
                    this.pokemonList.map(function(value, key) {
                        self.getPokemonListImg(value.url,key)
                    })
				})
				.catch(error => { 
					console.log(error)
				})
				.finally(() => (this.loading = false))
			},500)
		},
        getInfoPokemon(url,name){
            this.$parent.majData(name,url,'click')
        },
        getPokemonListSearch(search){
            const self = this
            this.noSearch = false
            this.loading = true

            setTimeout(()=>{
                axios.get(this.urlSearch)
                .then(response => {
                    //On vient mettre la liste à jour
                    this.pokemonList = response.data.results.filter(pokemonList =>
                        pokemonList.name.toLowerCase().includes(search.toLowerCase())
                    );

                    //on vient mettre à jour l'img pour chaque objet pokemon
                    //en appelant via l'url de chaque pokemon
                    this.pokemonList.map(function(value, key) {
                        self.getPokemonListImg(value.url,key)
                    })
                })
                .catch(error => { 
                    console.log(error)
                })
                .finally(() => (this.loading = false))
            },500)
        }
	},
	mounted () {
        //On lance la liste de pokemon de base
		this.getPokemonListFirst()
	}

}
</script>

<style scoped lang="scss">
    @import "https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css";

    .pokemon-item{
        // max-width: 75%;
        // margin: 50px auto;
        width: 100%;
        background: white;
        border-radius: 20px;
        padding: 50px;
        display: flex;
        flex-direction: column;
        row-gap: 20px;
        transition: all 400ms ease-in-out;
        ul.pokemon-list{
            list-style: none;
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            margin-bottom: 20px;
            li{
                display: flex;
                flex-direction: row;
                align-items: center;
                border-bottom: 1px solid rgb(134, 134, 134);
                cursor: pointer;
                padding: 20px;
                .image-container{
                    margin-right: 20px;
                    img{
                        transition: all 400ms ease-in-out;
                        width: 100%;
                    }
                }
                span{
                    text-transform: capitalize;
                    font-size: 18px;
                    transition: all 400ms ease-in-out;
                }
                i{
                    transition: all 400ms ease-in-out;
                    font-size: 18px;
                }
                &:hover{
                    .image-container{
                        img{
                            transform: scale(1.1);
                        }
                    }
                    span{
                        transform: translateX(30px);
                    }
                    i{
                        transform: translateX(30px);
                    }
                }
            }
        }
    }
	.loading{
		.loader{
			width: 200px;
			animation: spin 2s linear infinite;
			transform-origin: center;
		}
	}

	///////////////////////////
	/////   Keyframes   ///////
	///////////////////////////
	@keyframes spin { 
		100% { 
			-webkit-transform: rotate(360deg); 
			transform:rotate(360deg); 
		} 
	}
</style>
