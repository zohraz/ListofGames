<!-- StarRating.svelte -->
<script>
  export let rating = 0;
  const maxRating = 5;
  const fullStar = "★";
  const emptyStar = "☆";
  /*  this function calculates the number of full and empty stars to display for a given rating.*/
  function getStars() {
    /* The first variable, "fullStars", is defined as the number of full stars to display,
     rounded to the nearest integer, from a given "rating" parameter. If the "rating" parameter is not provided or if it is less than 0,
      "fullStars" will be equal to 0. */
    const fullStars = Math.round(rating) || 0;
    /*  The second variable, "emptyStars", is defined as the number of empty stars to display. It is calculated by subtracting the total number of stars, represented by the "maxRating" variable, 
    from the number of full stars, represented by the "fullStars" variable. */
    const emptyStars = maxRating - fullStars;
    /* Then, the function returns a string representing the stars to display. This string is constructed by concatenating two strings. The first string is a string representing the full stars, which is repeated "fullStars" times using JavaScript's "repeat" method.
     The second string is a string representing the empty stars, which is repeated "emptyStars" times in the same way. */
    return fullStar.repeat(fullStars) + emptyStar.repeat(emptyStars);
  }
</script>

<span>{getStars()}</span>
