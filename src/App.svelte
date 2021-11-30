<script>
	import { onMount } from 'svelte';

	let songs = [];

	let newSong = { title: '', artist: '' };

	const saveSong = () => {
		songs = [...songs, newSong];
		newSong = { title: '', artist: '' };
	};

	const deleteSong = (index) => {
		songs.splice(index, 1);
		songs = songs;
	};

	let works = [];

	onMount(async () => {
		const response = await fetch('https://api.artic.edu/api/v1/artworks?limit=10');
		const { data } = await response.json();
		console.log({ data });
		works = data.map(({ title, artist_title }) => ({ title, artist: artist_title }));
	})

</script>

<h1>Song List</h1>
<div id="song-list-container">
	<div id="new-song-container">
		<label>
			Title:&nbsp;
			<input placeholder="Title"bind:value={newSong.title}>
		</label>
		<label>
			Artist:&nbsp;
			<input placeholder="Artist" bind:value={newSong.artist}>
		</label>
		<button on:click={saveSong}>Save</button>
	</div>
	<ul>
		{#each songs as { title, artist }, index}
			<div id="song-list-item">
				<li><strong>Title:</strong> {title}, <strong>Artist:</strong> {artist}</li>
				<button on:click={() => deleteSong(index)}>Delete</button>
			</div>
		{/each}
	</ul>
</div>

<div>
	<h2>Chicago Art</h2>
	<ul>
		{#if works.length > 0}
			{#each works as { artist, title }}
				<li>{artist}, {title}</li>
			{/each}
		{:else}
			...loading
		{/if}
	</ul>
</div>

<style>
	#song-list-container {
		display: flex;
		flex-direction: column;
	}

	#new-song-container {
		display: flex;
		align-items: center;
	}

	#new-song-container label {
		margin: 0.5rem;
	}

	button {
		width: 6rem;
		height: 2rem;
	}

	#song-list-item {
		display: flex;
		align-items: center;
	}

	#song-list-item > * {
		margin: 0.5rem;
	}
</style>