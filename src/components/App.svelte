<script>
    import {onMount} from 'svelte';
    import Nav from "./Nav.svelte";
    import Dossier from "./Dossier.svelte";
    import FriendMap from "./FriendMap.svelte";

    let id;
    let data;

    $: window.location.hash = id ? id : '';

    async function hashChange() {
        if (id === '') return;

        data = await fetch(`https://darkstalker.herokuapp.com/api/v1/stalk/${id}`)
                .then(r => r.json())
                .then(r => {
                    if (r.hasOwnProperty('error')) {
                        throw new Error(r.error);
                    }

                    return r;
                })
                .catch(_ => null);
    }

    onMount(() => id = window.location.hash);
    onMount(hashChange);
</script>

<svelte:window on:hashchange={hashChange}/>

<Nav bind:value={id}/>

<main>
    <div class="dossier-container">
        {#if data}
            <Dossier user={data.user} friendsCount={data.friends.length}/>
        {/if}
    </div>
    <div class="friend-map-container">
        <FriendMap/>
    </div>
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