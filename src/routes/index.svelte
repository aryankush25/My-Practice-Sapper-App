<script context="module">
  export function preload(page) {
    return this.fetch(
      'https://my-practice-svelte-app.firebaseio.com/meetups.json',
    )
      .then(res => {
        if (!res.ok) {
          throw new Error('An error occured please try again')
        }
        return res.json()
      })
      .then(data => {
        const loadedMeetups = []

        for (const key in data) {
          loadedMeetups.push({
            ...data[key],
            id: key,
          })
        }

        return { fetchedMeetups: loadedMeetups.reverse() }
      })
      .catch(error => {
        this.error(500, 'Could Not Fetch')
      })
  }
</script>

<script>
  import { onMount, onDestroy } from 'svelte'
  import { scale } from 'svelte/transition'
  import { flip } from 'svelte/animate'
  import MeetUpItem from '../components/MeetUps/MeetUpItem.svelte'
  import MeetUpFilter from '../components/MeetUps/MeetUpFilter.svelte'
  import EditMeetup from '../components/MeetUps/EditMeetup.svelte'
  import Button from '../components/UI/Button.svelte'
  import meetupsStore from '../store/meetups-store.js'

  export let fetchedMeetups
  let loadedMeetups = []
  let favsOnly = false
  let isEditing = false
  let showDetails = false
  let currentId
  let editMeetUpId = null
  let unsubscribe

  onMount(() => {
    unsubscribe = meetupsStore.subscribe(items => {
      loadedMeetups = items
    })

    meetupsStore.setMeetups(fetchedMeetups)
  })

  onDestroy(() => {
    if (unsubscribe) unsubscribe()
  })

  const closeModal = () => {
    isEditing = !isEditing
    editMeetUpId = null
  }

  const handelShowDetails = ({ detail }) => {
    currentId = detail
    showDetails = true
  }

  const handelHideDetails = () => {
    currentId = null
    showDetails = false
  }

  const handelEditMeetupData = ({ detail }) => {
    isEditing = !isEditing
    editMeetUpId = detail
  }

  $: filteredMeetups = favsOnly
    ? loadedMeetups.filter(meetUp => meetUp.isFavourite)
    : loadedMeetups

  const setFilter = event => {
    favsOnly = event.detail
  }
</script>

<svelte:head>
  <title>All Meetups</title>
</svelte:head>

<Button on:click={() => (isEditing = !isEditing)}>Add Meetup</Button>

{#if isEditing}
  <svelte:component this={EditMeetup} on:close={closeModal} id={editMeetUpId} />
{/if}

<MeetUpFilter on:select={setFilter} {favsOnly} />

{#each filteredMeetups as meetup (meetup.id)}
  <div transition:scale animate:flip={{ duration: 300 }}>
    <MeetUpItem
      id={meetup.id}
      title={meetup.title}
      subtitle={meetup.subtitle}
      description={meetup.description}
      imageUrl={meetup.imageUrl}
      address={meetup.address}
      contactEmail={meetup.contactEmail}
      isFavourite={meetup.isFavourite}
      on:showDetails
      on:edit={handelEditMeetupData} />
  </div>
{/each}
