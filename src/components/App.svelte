<script>
    import {onMount} from 'svelte';
    import {fade} from 'svelte/transition';
    import Nav from "./Nav.svelte";
    import Dossier from "./Dossier.svelte";
    import FriendMap from "./FriendMap.svelte";
    import Documentation from "./Documentation.svelte"

    let id = '';
    let data = fetchData();

    $: window.location.hash = id ? id : '';

    async function fetchData() {
        return fetch(`https://darkstalker.herokuapp.com/api/v1/stalk/${id}`)
                .then(r => r.json())
                .then(r => {
                    if (r.hasOwnProperty('error')) {
                        throw new Error(r.error);
                    }

                    return r;
                })
                .catch(_ => null);
    }

    async function hashChange() {
        if (id === '') return;

        data = fetchData();
    }

    onMount(() => id = window.location.hash);
    onMount(hashChange);
</script>

<svelte:window on:hashchange={hashChange}/>

<Nav bind:value={id}/>

<main>
    {#if id === ''}
        <Documentation/>
    {:else}
        {#await data}
        {:then _data}
            <div transition:fade class="dossier-container">
                <Dossier user={_data.user} friendsCount={_data.friends.length}/>
            </div>
            <div class="friend-map-container">
                <FriendMap user={
            {id: _data.user.id,
             name:`${_data.user.first_name} ${_data.user.last_name}`,
             photo: _data.user.photo_100}
            } friends={_data.friends}
                           bind:value={id}/>
            </div>
        {/await}
    {/if}
</main>

<style>
    main {
        display: flex;
        flex-wrap: wrap;
        min-height: 100vh;
    }

    .dossier-container {
        flex: 1;
        width: 100%;
        max-width: 30vw;
        border-right: 1px solid #787878;
        background-color: #bdbdbd;
    }

    .friend-map-container {
        flex: 1;
        width: 100%;
        max-width: 70%;
    }

    h1 {
        color: #ff3e00;
        text-transform: uppercase;
        font-size: 4em;
        font-weight: 100;
    }

    @media (min-width: 640px) {
        main {
            max-width: none;
        }
    }
</style>