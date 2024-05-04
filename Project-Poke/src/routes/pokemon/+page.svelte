<script>
  import { onMount } from 'svelte';
  import { goto } from '$app/navigation';

  let pokemons = [];

  onMount(async () => {
    try {
      const response = await fetch('https://pokeapi.co/api/v2/pokemon?limit=10');
      const data = await response.json();
      
      pokemons = await Promise.all(
        data.results.map(async (pokemon) => {
          const pokemonDetails = await fetch(pokemon.url);
          const detailsData = await pokemonDetails.json();
          return {
            name: pokemon.name,
            image: detailsData.sprites.front_default, 
            url: pokemon.url,
          };
        })
      );
    } catch (error) {
      console.error('Erreur lors de la récupération des Pokémon:', error);
    }
  });

  function handlePokemonClick(pokemon) {
    goto(`/pokemon/${pokemon.name}`);
  }
</script>

<h1>Liste des Pokémon</h1>
<ul>
  {#each pokemons as pokemon}
    <li>
      <button on:click={() => handlePokemonClick(pokemon)}>
        <img src={pokemon.image} alt={`Image de ${pokemon.name}`} width="50" height="50" />
        {pokemon.name}
      </button>
    </li>
  {/each}
</ul>
