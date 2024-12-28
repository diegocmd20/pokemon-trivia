<script>
export default {
    name: "PokemonCard",
    props: {
        pokemon: Object,
    },
    data() {
        return {
            inputName: "",
            isDiscovered: false,
        };
    },
    methods: {
        checkName() {
            if (this.inputName.toLowerCase() === this.pokemon.name.toLowerCase()) {
                this.isDiscovered = true;
                this.$emit("pokemon-discovered");
            } else {
                alert("Nombre incorrecto. ¡Intenta de nuevo!");
            }
        },
    },
};
</script>

<template>
    <div class="pokemon-card">
        <img :src="pokemon.image" :class="{ blurred: !isDiscovered }" alt="Pokemon" />
        <div v-if="!isDiscovered">
            <input v-model="inputName" placeholder="Adivina el nombre" @keyup.enter="checkName" />
            <button @click="checkName">Descubrir</button>
        </div>
        <div v-else>
            <h3>{{ pokemon.name }}</h3>
        </div>
    </div>
</template>

<style>
.pokemon-card {
    background-color: var(--pokedex-white);
    color: var(--pokedex-black);
    border: 2px solid var(--pokedex-black);
    border-radius: 10px;
    text-align: center;
    padding: 15px;
    box-shadow: 0 4px var(--pokedex-gray);
    transition: transform 0.3s, box-shadow 0.3s;
}

.pokemon-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 6px var(--pokedex-black);
}

.pokemon-card img {
    width: 100px;
    height: 100px;
    margin: 10px auto;
    display: block;
    border-radius: 50%;
    border: 3px solid var(--pokedex-blue);
}

.blurred {
    filter: blur(5px) grayscale(80%);
}

input {
    width: 80%;
    padding: 8px;
    margin-top: 10px;
    font-size: 1rem;
    border: 2px solid var(--pokedex-yellow);
    border-radius: 5px;
    outline: none;
}

button {
    background-color: var(--pokedex-yellow);
    color: var(--pokedex-black);
    border: none;
    padding: 8px 15px;
    font-size: 1rem;
    font-weight: bold;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s;
}

button:hover {
    background-color: var(--pokedex-blue);
    color: var(--pokedex-white);
}
</style>
