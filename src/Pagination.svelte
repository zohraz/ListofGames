<script>
  import { createEventDispatcher } from "svelte";
  export let page = 1;
  export let pageSize = 20;
  export let totalResults = 0;
  const dispatch = createEventDispatcher();

  let currentPage = 1;
  let totalPages = 1;
  /* Le code dispatch("pageChange", currentPage) en Svelte JS permet de déclencher un événement personnalisé appelé "pageChange" et de lui passer en paramètre la valeur de la variable currentPage.

Les événements personnalisés sont utilisés pour permettre la communication entre les composants de l'application. Lorsqu'un événement personnalisé est déclenché, les autres composants qui y sont abonnés peuvent y réagir en effectuant une action spécifique.

Dans ce cas précis, le composant qui contient le code dispatch("pageChange", currentPage) peut déclencher l'événement "pageChange" pour signaler qu'une nouvelle page a été sélectionnée, et les autres composants qui écoutent cet événement peuvent y réagir en mettant à jour leur propre affichage ou en effectuant une autre action appropriée en fonction du contexte de l'application. */
  const handlePrevClick = () => {
    if (currentPage > 1) {
      currentPage--;
      dispatch("pageChange", currentPage);
    }
  };

  const handleNextClick = () => {
    if (currentPage < totalPages) {
      currentPage++;
      dispatch("pageChange", currentPage);
    }
  };

  /* The $: syntax in Svelte is a shorthand for a reactive statement, which means that it will re-run whenever any of its dependencies change. In other words, it creates a reactive declaration.

In the case of $: totalPages = Math.ceil(totalResults / pageSize);, this means that totalPages will be recalculated whenever either totalResults or pageSize changes. */
  $: totalPages = Math.ceil(totalResults / pageSize);

  $: if (page < 1) page = 1;
  $: if (page > totalPages) page = totalPages;
  $: currentPage = page;
  $: if (totalPages <= 1) currentPage = 1;

  $: if (currentPage === 1 && totalPages === 1) {
    dispatch("pageChange", currentPage);
  }

  $: if (currentPage === totalPages && totalPages !== 1) {
    dispatch("pageChange", currentPage);
  }
</script>

<div class="pagination">
  <button class="prev-button" on:click={handlePrevClick}> Prev </button>
  <span class="current-page">{currentPage}</span>
  <span class="total-pages">/ {totalPages}</span>
  <button class="next-button" on:click={handleNextClick}> Next </button>
</div>

<style>
  /* Style the pagination */
  .pagination {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 3rem;
    font-size: 1.2rem;
  }

  .prev-button,
  .next-button {
    border-radius: 5px;

    background-color: #e53637;
    color: #fff;
    padding: 0.5rem 1rem;
    margin: 0 0.5rem;
    cursor: pointer;
  }

  .prev-button:hover,
  .next-button:hover {
    background-color: #084298;
  }

  .prev-button:disabled,
  .next-button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }

  .current-page,
  .total-pages {
    margin: 0 0.5rem;
    color: wheat;
  }
</style>
