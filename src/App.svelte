<script>
  import { onMount } from "svelte";
  import GameCard from "./GameCard.svelte";
  import Modal from "./Modal.svelte";
  import "bootstrap/dist/css/bootstrap.min.css";
  import "bootstrap/dist/js/bootstrap.bundle.min.js";
  import Pagination from "./Pagination.svelte";
  export let totalPages = 10;
  let currentPage = 1;
  let totalPagesArray = [];
  const PAGE_SIZE = 20;
  /*   my API key from https://api.rawg.io/ */
  const apiKey = "aa605dea2f214ea1a5d53faca896705b";
  let games = [];
  let filteredGames = [];
  let searchTerm = "";
  const cacheKey = "gameData";
  let isLoading = true;
  async function handlePageChange(page) {
    //console.log(page);
    currentPage = page.detail;
    filteredGames = await fetchGames(page.detail);
  }

  onMount(async () => {
    // when you want Clear the  cache
    //localStorage.removeItem(cacheKey);
    for (let i = 1; i <= totalPages; i++) {
      totalPagesArray.push(i);
    }
    // Call this when the component is mounted to load the previously saved datagame
    const cachedData = localStorage.getItem(cacheKey);
    // Check if data is in cache
    if (cachedData) {
      //console.log("data from cache");
      games = JSON.parse(cachedData);
      filteredGames = JSON.parse(cachedData);
      isLoading = false;
    } else {
      // Fetch data from API if not in cache
      console.log("data from api");
      await loadGames(currentPage);
    }
    // Use filteredGames in your component
    console.log(filteredGames);
  });

  /*   Get list of games from api rawg
   * @param page
   * @returns */
  async function fetchGames(page) {
    const response = await fetch(
      `https://api.rawg.io/api/games?key=${apiKey}&page=${page}&page_size=${PAGE_SIZE}`
    );
    const data = await response.json();
    return data.results;
  }

  /*   Search a game with name or tag
   * @param searchTerm
   * @returns */
  async function searchGames(searchTerm) {
    const response = await fetch(
      `https://api.rawg.io/api/games?key=${process.env.RAWG_API_KEY}&search=${searchTerm}`
    );
    const data = await response.json();
    return data.results;
  }
  /*   call fetchGames to load the data
   * @param page
   * @returns */
  async function loadGames(page) {
    games = await fetchGames(page);
    filteredGames = games;
    // Store data in cache
    localStorage.setItem(cacheKey, JSON.stringify(filteredGames));
    isLoading = false;
  }

  /*The function filterGames() filters games based on a searchTerm searchTerm */
  function filterGames() {
    /*The function starts by filtering the list of games (games) using the filter() method.
 The filter() method returns a new array containing all the elements of the original array (games) that satisfy the given condition.*/
    filteredGames = games.filter((game) => {
      /*  For each game in games, the function uses the toLowerCase() method to convert its name to lowercase, then uses the includes() method to check if the search term (searchTerm) is included in the game name. The result of this comparison is stored in the nameMatches variable.*/
      const nameMatches = game.name
        .toLowerCase()
        .includes(searchTerm.toLowerCase());
      /* The function then uses the some() method to iterate through all the tags for each game and check if at least one tag matches the search term. Similar to the previous step, the comparison is performed by converting the tag name to lowercase using the toLowerCase() method, then checking if the search term is included in the tag name using using the includes() method. The result of this comparison is stored in the tagsMatch variable.*/
      const tagsMatch = game.tags.some((tag) =>
        tag.name.toLowerCase().includes(searchTerm.toLowerCase())
      );
      /*If nameMatches or tagsMatch is true (the search term matches the name of the game or a tag), then that game is added to a new array called filteredGames.*/
      return nameMatches || tagsMatch;
    });
    /* Finally, the function resets the currentPage variable to 1, which means that paging starts again from the first page.*/
    currentPage = 1;
  }
  /*   handleSearchInput is a function that is typically used as an event handler for handling user input in a search field. 
The function takes an event object as its parameter, which contains information about the user's input
and call filterGames to search with this searchterm
   * @param event */
  function handleSearchInput(event) {
    searchTerm = event.target.value;
    filterGames();
  }
</script>

<main>
  {#if isLoading}
    <div class="spinner">
      <h2>Chargement des données from Api rawg/cache</h2>
    </div>
  {/if}
  <div class="container">
    <div class="row">
      <div class="col-lg-3">
        <div class="section-title">
          <div class="games__sidebar">
            <div class="games__sidebar__view">
              <div class="section-title">
                <h5>List of games</h5>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <input
      type="text"
      placeholder="Find a game..."
      on:input={handleSearchInput}
    />
    <div class="games-container">
      {#each filteredGames as game}
        <GameCard {game} />
      {/each}
    </div>
    <Pagination
      page={currentPage}
      totalResults={10000}
      pageSize={PAGE_SIZE}
      on:pageChange={handlePageChange}
    />
  </div>
</main>

<style>
  @import "bootstrap/dist/css/bootstrap.css";
  /*---------------------
  Games
-----------------------*/
  /*---------------------
 The games should be displayed in a responsive grid with four columns.
The grid should be displayed in a container of maximum 1440px
-----------------------*/
  .games-container {
    max-width: 1440px;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-gap: 20px;
  }
  /*---------------------
 The games should be displayed in a responsive grid  two columns on mobile devices.
-----------------------*/
  @media (max-width: 767px) {
    .games-container {
      grid-template-columns: repeat(2, 1fr);
    }
  }
  /* Style the spinner */

  .spinner {
    display: inline-block;
    width: 20px;
    height: 20px;
    border-radius: 50%;
    border: 2px solid #ccc;
    border-top-color: #333;
    animation: spin 1s ease-in-out infinite;
  }

  @keyframes spin {
    to {
      transform: rotate(360deg);
    }
  }
  /* Style the container */
  .games__sidebar .section-title h5 {
    color: #ffffff;
    font-weight: 600;
    font-family: "Oswald", sans-serif;
    line-height: 21px;
    text-transform: uppercase;
    padding-left: 20px;
    position: relative;
  }
  .games__sidebar .section-title h5:after {
    position: absolute;
    left: 0;
    top: -6px;
    height: 32px;
    width: 4px;
    background: #e53637;
    content: "";
  }
  .games__sidebar__view {
    position: relative;
    margin-bottom: 80px;
  }
  .container {
    margin: 0 0;
    padding: 20px;
    font-family: Arial, sans-serif;
  }
  /* Style the search input */
  input[type="text"] {
    width: 100%;
    padding: 10px;
    margin-bottom: 20px;
    font-size: 1rem;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
  }
</style>
