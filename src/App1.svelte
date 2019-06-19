<script>
  import { onMount } from "svelte";
  import hobbyStore from "./hobby-store.js";

  let hobbies = [];
  let hobbyInput;
  let isLoading = false;

  isLoading = true;
  onMount(() => {
    fetch("https://svelte-course-123.firebaseio.com/hobbies.json")
      .then(res => {
        isLoading = false;
        if (!res.ok) {
          throw new Error("Failed!");
        }
        return res.json();
      })
      .then(data => {
        // hobbies = Object.values(data);
        hobbyStore.setHobbies(Object.values(data));
      })
      .catch(err => {
        isLoading = false;
        console.log(err);
      });
  });

  function addHobby() {
    // hobbies = [...hobbies, hobbyInput.value];
    hobbyStore.addHobbies(hobbyInput.value);

    isLoading = true;
    fetch("https://svelte-course-123.firebaseio.com/hobbies.json", {
      method: "POST",
      body: JSON.stringify(hobbyInput.value),
      headers: {
        "Content-Type": "application/json"
      }
    })
      .then(res => {
        isLoading = false;
        if (!res.ok) {
          throw new Error("Failed!");
        }
        //....
        // alert("Saved data");
      })
      .catch(err => {
        isLoading = false;
        console.log(err);
      });
  }
</script>

<label for="hobby">Hobby</label>
<input type="text" id="hobby" bind:this={hobbyInput} />
<button on:click={addHobby}>Add hobby</button>

{#if isLoading}
  <p>Loading....</p>
{:else}
  <ul>
    {#each $hobbyStore as hobby}
      <li>{hobby}</li>
    {/each}
  </ul>
{/if}

<!-- {#await getHobbies}
  <p>Loading...</p>
{:then hobbyData}
  <ul>
    {#each hobbyData as hobby}
      <li>{hobby}</li>
    {/each}
  </ul>
{:catch error}
  <p>error.message</p>
{/await} -->
