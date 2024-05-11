<script>
  import { onMount } from 'svelte';
  import { page } from '$app/stores';

  let pokemon;
  let pokemonName = $page.params.name; 

  async function fetchPokemonDetails(name) {
    const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${name}`);
    const data = await response.json();
    pokemon = data; 
  }

  onMount(() => {
    fetchPokemonDetails(pokemonName);
  });

  function getColor(stat) {
    // les couleurs basées sur la valeur des statistiques
    if (stat >= 75) return 'green'; 
    if (stat >= 50) return 'orange'; 
    return 'red'; 
  }
</script>

{#if pokemon}
<h1 class="infos-titre">Infos Pokémon</h1>
  <div class="pokemon-card">
    <h1 class="pokemon-name">{pokemon.name}</h1> 
    <img class="pokemon-image" src={pokemon.sprites.front_default} alt={`Image de {pokemon.name}`} />

    <div class="pokemon-info">
      <h2>Stats</h2>
      <ul>
        {#each pokemon.stats as stat}
          <li class="stat-item">
            <span class="stat-name">{stat.stat.name}:</span>
            <div class="progress-bar">
              <div class="progress" style="width: {Math.min(stat.base_stat, 100)}%; background-color: {getColor(stat.base_stat)};"></div>
            </div>
            <span class="stat-value">{stat.base_stat}%</span>
          </li>
        {/each}
      </ul>

      <h2>Types</h2>
      <ul>
        {#each pokemon.types as type}
          <li class="type-item">{type.type.name}</li>
        {/each}
      </ul>
    </div>
  </div>
{/if}

<!-- Déburt de style  -->
<style>
  .infos-titre {
    color: #4d1d00;
    font-weight: 800;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
    width: 100%;
    position: relative;
    left: 20px;
  }
  .pokemon-info{
    position: relative;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
    padding: 20px;
    width: 350px;
    border-radius: 10px;
  }

  .pokemon-card {
    display: flex;
    position: relative;
    width: 100%;
    justify-content: space-around;
    border: 1px solid #f1eeee; 
    border-radius: 10px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3); 
    padding: 20px;
    background-color: rgb(255, 255, 255); 
    text-align: center; 
  }

  .pokemon-name {
    text-transform: capitalize; 
    margin-bottom: 1px; 
    position: relative;
    align-items: center;
    top: 100px;
  }

  .pokemon-image {
    position: relative;
    align-items: center;
    top: 100px;
    right: 100px;
    width: 100px; 
    height: 100px; 
    border-radius: 10px; 
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
  }

  .pokemon-info {
    text-align: left; 
  }

  .progress-bar {
    width: 100px; 
    height: 13px; 
    background-color: #e0e0e0; 
    border-radius: 3px; 
    overflow: hidden; 
  }

  .progress {
    height: 100%; 
    border-radius: 5px; 
    transition: width 0.3s; 
  }

  .stat-item {
    display: flex; 
    align-items: center; 
    gap: 10px; 
  }

  .stat-name {
    text-transform: capitalize;
  }

  .stat-value {
    font-weight: bold; 
  }

  .type-item {
    text-transform: capitalize; 
  }
</style>
<!-- Fin de style  -->
