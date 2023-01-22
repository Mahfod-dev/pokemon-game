<template>
	<h1 v-if="!pokemon">Attendez svp</h1>

	<div v-else>
		<h1>Quelle est le pokemon ?</h1>
		<PokemonPicture :pokemon-id="pokemon?.id" :show-pokemon="showPokemon" />
		<PokemonOptions :pokemons="pokemonArr" @pokemon-selection="checkAnswer" />
	</div>

	<template v-if="showAnswer">
		<h2 class="fade-in">{{ message }}</h2>
		<button @click="newGame">Nouveau jeu</button>
	</template>
</template>

<script>
	import PokemonOptions from '@/components/PokemonOptions.vue';
	import PokemonPicture from '@/components/PokemonPicture.vue';
	import getPokemonOption from '@/helpers/getPokemonOptions';

	export default {
		components: {
			PokemonOptions,
			PokemonPicture,
		},
		data() {
			return {
				pokemonArr: [],
				pokemon: null,
				showPokemon: false,
				showAnswer: false,
				message: '',
			};
		},
		methods: {
			async mixPokemonArray() {
				this.pokemonArr = await getPokemonOption();

				const rndInt = Math.floor(Math.random() * 4);

				this.pokemon = this.pokemonArr[rndInt];
			},

			checkAnswer(selectedId) {
				this.showPokemon = true;
				this.showAnswer = true;

				if (selectedId === this.pokemon.id) {
					this.message = `Correct, ${this.pokemon.name}`;
				} else {
					this.message = `Oops, era ${this.pokemon.name}`;
				}
			},
		},
		newGame() {
			console.log('hello');
			this.showPokemon = false;
			this.showAnswer = false;
			this.pokemonArr = [];
			this.pokemon = null;
			this.mixPokemonArray();
		},
		mounted() {
			this.mixPokemonArray();
		},
	};
</script>

<style lang="scss" scoped></style>
