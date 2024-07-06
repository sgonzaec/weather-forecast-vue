<script>
export default {
  name: "NoteAlert",
  // resto del código
};
</script>

<template>
  <div class="alert">
    <h3>Alert:</h3>
    <p>Search engine autocompletion is inactive due to Google Maps costs</p>
  </div>
</template>

<style scoped>
  .alert {    
    width: 25em;
    background-color: white;
    padding: 0 1em;
    display: flex;
    margin-top: 2em;
    border-radius: 15px;
    flex-direction: column;
    align-items: center;
    box-shadow: 0px 0px 10px 0px rgba(0, 0, 0, 0.25);
    position: fixed;
    right: 1em;
    bottom: 4em;
  }
  h3 {
    margin: 1em 0 0 0;
  }
</style>
