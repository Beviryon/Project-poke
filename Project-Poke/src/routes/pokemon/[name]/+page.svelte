<script context="module">
    export async function load({ params }) {
      const { name } = params;
  
      if (!name) {
        throw new Error("Le nom du Pokémon n'a pas été fourni.");
      }
  
      const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${name}`);
      if (!response.ok) {
        throw new Error(`Erreur lors de la récupération des détails du Pokémon : ${response.statusText}`);
      }
  
      const pokemon = await response.json();
      return { pokemon };  
    }
  </script>
  
  
  
  <script>
    export let pokemon;
  </script>
  
  {#if pokemon}
  <h1>Détails du Pokémon : {pokemon.name}</h1>
  <img src={pokemon.sprites.front_default} alt={`Image de ${pokemon.name}`} />
  <p>Identifiant : {pokemon.id}</p>
  <p>Expérience de base : {pokemon.base_experience}</p>
  <p>Hauteur : {pokemon.height}</p>
  <p>Poids : {pokemon.weight}</p>
  <p>Types : {pokemon.types.map(t => t.type.name).join(", ")}</p>
  <p>Capacités : {pokemon.abilities.map(a => a.ability.name).join(", ")}</p>
{:else}
  <p>Loading...</p>
{/if}
