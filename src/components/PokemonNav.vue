<template>
	<div class="pokemon-nav">
		<img class="pokemon-item__image" :src="image.source" :alt="image.alt" />
	</div>
</template>

<script>
export default {
	name: "PokemonNav",
	props: ["name", "image"]
}
</script>

<style scoped lang="scss">
    .pokemon-nav{
        width: 200px;
        height: 200px;
        border-radius: 200px;
        display: flex;
        background-color: white;
        overflow: hidden;
        cursor: pointer;
        border: 3px solid #3264af;
        transition: all 400ms ease-in;
        img{
            transition: all 400ms ease-in;
            width: 100%;
        }
        &:hover{
            opacity: 0.7;
            img{
                transform: scale(1.2);
            }
        }
        &.active{
            border: 3px solid #17285e;
        }
    }
    
	///////////////////////////
	/////   MediaQueries //////
	///////////////////////////
	@media screen and (max-width: 1100px) {
		.pokemon-nav{
            width: 150px;
            height: 150px;
        }
	}

	@media screen and (max-width: 500px) {
		.pokemon-nav{
            width: calc(100% / 3);
            height: auto;
        }
	}
</style>
