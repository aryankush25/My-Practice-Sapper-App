<script context="module">
  export function preload(page) {
    const meetUpId = page.params.id

    return this.fetch(
      `https://my-practice-svelte-app.firebaseio.com/meetups/${meetUpId}.json`,
    )
      .then(res => {
        if (!res.ok) {
          throw new Error('An error occured please try again')
        }
        return res.json()
      })
      .then(data => {
        console.log('$$$$ data', data)
        return { loadedMeetups: { ...data, id: meetUpId } }
      })
      .catch(error => {
        this.error(500, 'Could Not Fetch')
      })
  }
</script>

<script>
  import Button from '../components/UI/Button.svelte'

  export let loadedMeetups
</script>

<style>
  section {
    margin-top: 4rem;
  }

  .image {
    width: 100%;
    height: 25rem;
  }

  img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .image {
    background: #e7e7e7;
  }

  .content {
    text-align: center;
    width: 80%;
    margin: auto;
  }

  h1 {
    font-size: 2rem;
    font-family: 'Roboto Slab', sans-serif;
    margin: 0.5rem 0;
  }

  h2 {
    font-size: 1.25rem;
    color: #6b6b6b;
  }

  p {
    font-size: 1.5rem;
  }
</style>

<section>
  <div class="image">
    <img src={loadedMeetups.imageUrl} alt={loadedMeetups.title} />
  </div>
  <div class="content">
    <h1>{loadedMeetups.title}</h1>
    <h2>{loadedMeetups.subtitle} - {loadedMeetups.address}</h2>
    <p>{loadedMeetups.description}</p>
    <Button href="mailto:{loadedMeetups.contactEmail}">Contact</Button>
    <Button href="/">Close</Button>
  </div>
</section>
