<script>
  // @ts-nocheck

  import Modal from "./Modal.svelte";
  export let game;
  export let showModal = false;

  const handleClick = () => {
    showModal = true;
  };

  function handleCloseModal() {
    showModal = false;
  }
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<div class="game-card col-lg-12 col-md-12 col-sm-12" on:click={handleClick}>
  <div class="game__item">
    <div
      class="game__item__pic set-bg"
      data-setbg={game.background_image}
      style="background-image: url({game.background_image}); background-size: cover;
      background-repeat: no-repeat;"
    >
      {#if game.metacritic}
        <div class="metatritic">
          Metacritic: <span>{game.metacritic}</span>
        </div>
      {:else}
        <p>No score available</p>
      {/if}
    </div>
    <div class="game__item__text">
      <ul>
        {#each game.platforms as platform}
          <li>{platform.platform.name}</li>
        {/each}
      </ul>

      <ul>
        {#each game.tags.slice(0, 2) as tag}
          <li>#{tag.name}</li>
        {/each}
      </ul>

      <h5>
        <!-- svelte-ignore a11y-invalid-attribute -->
        <a href="#" on:click={handleClick}>{game.name}</a>
      </h5>
    </div>
  </div>
  {#if showModal}
    <Modal {game} isOpen={showModal} on:close={handleCloseModal} />
  {/if}
</div>

<style>
  /*---------------------
  Product
-----------------------*/
  .game__item {
    margin-bottom: 30px;
  }

  .game__item__pic {
    height: 325px;
    position: relative;
    border-radius: 5px;
  }
  @media (min-width: 576px) {
    .game__item__text {
      flex-direction: row;
      justify-content: space-between;
    }

    .game__item__text ul {
      margin-right: 1rem;
    }
  }
  .metatritic {
    font-size: 13px;
    color: #ffffff;
    background: #e53637;
    display: inline-block;
    padding: 2px 12px;
    border-radius: 4px;
    position: absolute;
    left: 10px;
    top: 10px;
  }
  .game__item__text {
    padding-top: 20px;
  }

  .game__item__text ul {
    margin-bottom: 10px;
  }

  .game__item__text ul li {
    list-style: none;
    font-size: 10px;
    color: #ffffff;
    font-weight: 700;
    padding: 1px 10px;
    background: rgba(255, 255, 255, 0.2);
    border-radius: 50px;
    display: inline-block;
  }

  .game__item__text h5 a {
    color: #ffffff;
    font-weight: 700;
    line-height: 26px;
    text-decoration: none;
    background-color: transparent;
  }
  .game-card:hover {
    box-shadow: 0 14px 18px #0080ff;
    opacity: 1;
    visibility: visible;
  }

  .product__sidebar__view__item h5 a {
    color: #ffffff;
    font-weight: 700;
    line-height: 26px;
  }

  .product__sidebar__comment__item__text ul li {
    list-style: none;
    font-size: 10px;
    color: #ffffff;
    font-weight: 700;
    padding: 1px 10px;
    background: rgba(255, 255, 255, 0.2);
    border-radius: 50px;
    display: inline-block;
  }

  .product__sidebar__comment__item__text h5 a {
    color: #ffffff;
    font-weight: 700;
    line-height: 26px;
  }
</style>
