<script>
  import meetups from './Meetups/meetups-store.js';

  import Header from "./UI/Header.svelte";
  import MeetupGrid from "./Meetups/MeetupGrid.svelte";
  import TextInput from "./UI/TextInput.svelte";
  import Button from "./UI/Button.svelte";
  import EditMeetup from "./Meetups/EditMeetup.svelte";

  // let meetups = ;

  let editMode = null;

  function addMeetup(event) {
    const meetupData = {
      title: event.detail.title,
      subtitle: event.detail.subtitle,
      description: event.detail.description,
      contactEmail: event.detail.email,
      address: event.detail.address,
      imageUrl: event.detail.imageUrl
    };

    meetups.addMeetup(meetupData)
    editMode = null;
  }

  function toggleFavorite(event) {
    const id = event.detail;
    meetups.toggleFavorite(id);
  }

  function cancelEdit() {
    editMode = null;
  }
</script>

<style>
  main {
    margin-top: 5rem;
  }

  .meetup-controls {
    margin: 1rem;
  }
</style>

<Header />

<main>
  <div class="meetup-controls">
    <Button on:click={() => (editMode = 'add')}>New meetup</Button>
  </div>

  {#if editMode === 'add'}
    <EditMeetup on:save={addMeetup} on:cancel={cancelEdit} />
  {/if}

  <MeetupGrid meetups={$meetups} on:togglefavorite={toggleFavorite} />

</main>
