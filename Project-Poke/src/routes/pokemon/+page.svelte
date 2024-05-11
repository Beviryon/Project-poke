<script>
  import { onMount } from 'svelte';
  import { goto } from '$app/navigation';

  let pokemons = [];
  let displayedPokemons = [];
  let searchQuery = '';
  let currentStartIndex = 0;
  const itemsPerPage = 20; 

  async function fetchPokemons() {
    try {
      const response = await fetch('https://pokeapi.co/api/v2/pokemon?limit=100');
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
      
      updateDisplayedPokemons();
    } catch (error) {
      console.error('Error fetching Pokémon:', error);
    }
  }

  function updateDisplayedPokemons() {
    const filteredPokemons = pokemons.filter(pokemon => 
      pokemon.name.toLowerCase().includes(searchQuery.toLowerCase())
    );
    displayedPokemons = filteredPokemons.slice(currentStartIndex, currentStartIndex + itemsPerPage);
  }

  onMount(fetchPokemons);

  function handlePokemonClick(pokemon) {
    goto(`/pokemon/${pokemon.name}`);
  }

  function nextPage() {
    if (currentStartIndex + itemsPerPage < pokemons.length) {
      currentStartIndex += itemsPerPage;
      updateDisplayedPokemons();
    }
  }

  function previousPage() {
    if (currentStartIndex > 0) {
      currentStartIndex -= itemsPerPage;
      updateDisplayedPokemons();
    }
  }

  function handleSearch(event) {
    searchQuery = event.target.value;
    currentStartIndex = 0; 
    updateDisplayedPokemons();
  }
</script>

<!-- Déburt de style  -->
<style lang="css"> 
  h1 {
    font-family: 'Arial', sans-serif;
    font-size: 2em;
    color: #333;
  }

  .pokemon-galerie {
    display: grid;
    grid-template-columns: repeat(5, 1fr); 
    gap: 10px; 
  }

  .pokemon-item {
    list-style-type: none;
  }

  .pokemon-button {
    display: flex;
    flex-direction: column; 
    align-items: center;
    background-color: #f8f9fa;
    border: none;
    padding: 10px;
    border-radius: 8px;
    box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
    transition: background-color 0.3s, transform 0.3s;
    cursor: pointer;
  }

  .pokemon-button:hover {
    background-color: #e9ecef;
    transform: translateY(-2px);
  }

  .pokemon-image {
    border-radius: 8px;
    width: 80px; 
    height: 80px;
  }

  .navigation-buttons {
    display: flex;
    justify-content: space-around;
    margin: 10px 0;
    width: 50%;
    left: 22.5%;
    position: relative;
    top: 20px;
  }

  .navigation-buttons button {
    background-color: #035e80;
    border: none;
    padding: 8px;
    color: #fff;
    border-radius: 8px;
    transition: 0.5s;
  }

  .navigation-buttons button:hover {
    background-color: #033f55;
  }

  .barre-recherche {
    margin: 10px 0;
    padding: 10px;
    width: 100%;
    border: 1px solid #ddd;
    border-radius: 8px;
    box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
  }
</style>
<!-- Fin de style -->

<h1>Liste des Pokémons</h1>

<input class="barre-recherche" type="text"placeholder="Rechercher un Pokémon..." on:input={handleSearch}/>

<ul class="pokemon-galerie">
  {#each displayedPokemons as pokemon}
    <li class="pokemon-item">
      <button class="pokemon-button" on:click={() => handlePokemonClick(pokemon)}>
        <img class="pokemon-image" src={pokemon.image} alt={`Image de ${pokemon.name}`} />
        {pokemon.name}
      </button>
    </li>
  {/each}
</ul>

<div class="navigation-buttons">
  <button on:click={previousPage} disabled={currentStartIndex === 0}>Précédent</button>
  <button on:click={nextPage} disabled={currentStartIndex + itemsPerPage >= pokemons.length}>Suivant</button>
</div>
