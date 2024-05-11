Ce projet est une application simple qui utilise Svelte pour afficher une liste de pokémons, permet la recherche et la navigation entre les pages. 

Décortiquons le code étape par étape :
-- scr/routes/pokemon/+page.svelte

- les importations du projet:
    import { onMount } from 'svelte';
    import { goto } from '$app/navigation';

- les variable du projet:
    let pokemons = [];
    let displayedPokemons = [];
    let searchQuery = '';
    let currentStartIndex = 0;
    const itemsPerPage = 20;

    pokemons --> est un tableau qui stockera tous les pokémons récupérés de l'API.
    displayedPokemons --> est un tableau qui stockera les pokémons actuellement affichés sur la page, en fonction du filtrage et de la pagination.
    searchQuery --> stocke la chaîne de caractères de recherche saisie par l'utilisateur.
    currentStartIndex --> indique l'index du premier pokémon à afficher dans la page actuelle.
    itemsPerPage définit le nombre de pokémons à afficher par page.

- Fonction fetchPokemons:
    Cette fonction récupère les données des pokémons depuis l'API Pokédex.
    Elle utilise fetch pour obtenir la liste des 100 premiers pokémons de l'API.
    Ensuite, elle utilise Promise.all pour récupérer les détails de chaque pokémon individuellement.
    Les détails de chaque pokémon incluent son nom, l'URL de son image et l'URL de sa page détaillée.
    Enfin, elle met à jour le tableau pokemons avec les données récupérées et appelle la fonction updateDisplayedPokemons pour initialiser l'affichage.

- Fonction updateDisplayedPokemons:
    Cette fonction filtre les pokémons en fonction de la chaîne de recherche et de l'index de départ actuel.
    Elle utilise filter pour créer un nouveau tableau contenant uniquement les pokémons dont le nom correspond à la recherche (en ignorant la casse).
    Ensuite, elle utilise slice pour extraire un sous-tableau des pokémons filtrés, en fonction de l'index de départ et du nombre d'éléments par page.
    Enfin, elle met à jour le tableau displayedPokemons avec les pokémons filtrés et paginés.

- Fonction handlePokemonClick:
    Cette fonction est appelée lorsque l'utilisateur clique sur un bouton de pokémon.
    Elle utilise goto pour naviguer vers une nouvelle page de détails du pokémon, en passant le nom du pokémon comme paramètre d'URL.

- Fonctions nextPage et previousPage:
    Les fonctions nextPage et previousPage gèrent la navigation entre les pages de pokémons dans l'application.
    Elles mettent à jour l'index de départ actuel (currentStartIndex) et appellent la fonction updateDisplayedPokemons pour recharger les pokémons à afficher sur la page en conséquence

-- scr/routes/pokemon/[name]+page.svelte
- Fonction getColor:
    Cette fonction prend une valeur de statistique en entrée et renvoie une couleur en fonction de la valeur.
    Elle utilise une logique simple pour attribuer le vert aux statistiques élevées (75 et plus), l'orange aux statistiques moyennes (50-74) et le rouge aux statistiques faibles (en dessous de 50). Cette fonction peut être personnalisée pour des attributions de couleurs plus nuancées en fonction de la distribution des statistiques.

- Contenu:

    Le code affiche les informations suivantes sur le Pokémon :

        Nom (avec la première lettre en majuscule)
        Image
        Statistiques :
        Nom et valeur de la statistique
        Barre de progression représentant la valeur de la statistique avec une couleur basée sur la fonction getColor
        
    Types :
        Liste des types auxquels appartient le Pokémon (avec la première lettre en majuscule)

    Style:
        Les styles inclus définissent l'apparence visuelle des éléments de la page, tels que les polices, les couleurs, la disposition et le positionnement.



---------------------------------------------------------------------------------------
# create-svelte

Everything you need to build a Svelte project, powered by [`create-svelte`](https://github.com/sveltejs/kit/tree/main/packages/create-svelte).

## Creating a project

If you're seeing this, you've probably already done this step. Congrats!

```bash
# create a new project in the current directory
npm create svelte@latest

# create a new project in my-app
npm create svelte@latest my-app
```

## Developing

Once you've created a project and installed dependencies with `npm install` (or `pnpm install` or `yarn`), start a development server:

```bash
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

## Building

To create a production version of your app:

```bash
npm run build
```

You can preview the production build with `npm run preview`.

> To deploy your app, you may need to install an [adapter](https://kit.svelte.dev/docs/adapters) for your target environment.
