<script>
    import Icon from 'svelte-awesome';
    import {github} from 'svelte-awesome/icons';

    import Logo from "./Logo.svelte";

    export let value = '';
</script>
<style>
    nav {
        display: flex;
        align-items: center;
        justify-content: space-between;
        width: 100vw;
        flex-direction: row;
        background-color: black;
        color: white;
    }

    .type-container {
        flex: 1;
        max-width: 640px;
        margin: 0 auto;
        width: 100%;
    }

    #type {
        margin: 0;
        width: 100%;
    }

    a {
        color: white;
    }

    a:hover {
        color: gray;
    }
</style>

<header>
    <nav>
        <Logo/>
        <div class="type-container">
            <label>
                <input bind:value={value} id="type" placeholder="Type VK user ID here...">
            </label>
        </div>
        <a href="https://github.com/cochegarka/darkstalker-frontend" target="_blank">
            <Icon style="margin: 0px 8px;" data={github} scale="2"/>
        </a>
    </nav>
</header>