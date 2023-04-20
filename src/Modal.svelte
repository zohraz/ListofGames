<script>
  import { onMount, onDestroy } from "svelte";
  import Carousel from "svelte-carousel";
  import StarRating from "./StarRating.svelte";

  export let isOpen;
  // export let closeModal = () => {};
  export let game = {};
  const closeModal = () => {
    isOpen = false;
  };
  let carousel; // for calling methods of the carousel instance
  // function use to close the modal when a click event is lunched outside of content modal
  function handleWindowClick(event) {
    if (isOpen && !event.target.closest(".modal__content")) {
      closeModal();
    }
  }
</script>

{#if isOpen}
  <!-- svelte-ignore a11y-click-events-have-key-events -->
  <div class="modal" on:click={handleWindowClick}>
    <div
      class="modal__content"
      style="border-style: groove;
      border-radius: 5%;"
    >
      <div class="modal__header">
        <div class="modal-title">
          <span style="color: white;">{game.name}</span>
        </div>
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <span class="close" on:click={closeModal}>&times;</span>
      </div>
      <div class="modal__body">
        <div class="modal__image">
          <img src={game.background_image} alt={game.name} />
        </div>
        <div class="modal__details">
          <div class="modal__platforms">
            <h3>Platforms:</h3>
            <div class="game__item__text">
              <ul>
                {#each game.platforms as platform}
                  <li>{platform.platform.name}</li>
                {/each}
              </ul>
            </div>
          </div>
          <div class="modal__rating">
            <h3>Rating:</h3>
            <StarRating rating={game.rating} />
          </div>
          <div>
            <h3>Screenshots:</h3>
          </div>
        </div>
        <Carousel
          bind:this={carousel}
          let:loaded
          autoplay
          autoplayDuration={3000}
          autoplayProgressVisible
        >
          {#each game.short_screenshots as screen}
            <img src={screen.image} alt={screen.image} />
          {/each}
        </Carousel>
      </div>
    </div>
  </div>
{/if}

<style>
  /* Style the modal */
  .game__item__text {
    padding-top: 20px;
  }
  .game__item__text ul {
    margin-bottom: 10px;
  }
  .game__item__text ul li {
    list-style: none;
    font-size: 10px;
    color: black;
    font-weight: 700;
    padding: 1px 10px;
    background: rgb(31 27 113 / 20%);
    border-radius: 50px;
    display: inline-block;
  }
  .modal {
    display: flex;
    justify-content: center;
    align-items: center;
    position: fixed;
    top: 0;

    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
  }
  .modal__content {
    background-color: white;
    border-radius: 10px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.5);
    max-width: 50%;
    max-height: 90%;
    position: absolute;
    overflow-y: scroll;
  }
  .modal__header {
    padding: 15px;
    border-bottom: 1px solid #e5e5e5;
    background: #e53637;
    flex-direction: row;
    height: 60px;
  }
  .close {
    position: absolute;
    top: 0;
    right: 0;
    padding: 10px;
    cursor: pointer;
    color: #000;
    text-shadow: 0 1px 0 #fff;
    opacity: 0.2;
  }
  .modal__body {
    display: flex;
    flex-wrap: wrap;
    padding: 10px;
  }
  .modal__image {
    display: block;
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
  .modal__image img {
    width: 100%;
    height: auto;
    object-fit: contain; /* ou object-fit: cover si vous voulez remplir tout l'espace disponible */

    border: none;
    border-radius: 10%;

    border-width: 1px;
    border-style: groove;
  }
  .modal__details {
    flex: 1 1 100%;
    margin-left: 10px;
  }
  .modal__platforms ul {
    margin: 0;
    padding: 0;
  }
  .modal__platforms li {
    list-style: none;
    margin-bottom: 5px;
  }
  .modal__rating {
    margin-top: 10px;
  }
</style>
