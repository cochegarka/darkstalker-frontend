<script>
    import {onMount} from 'svelte';
    import {Network} from 'vis-network';

    export let value = '';

    export let user;
    export let friends;

    let network;

    onMount(async () => {
        let nodes = [];

        let pushNode = record => {
          nodes.push({id: record.id, shape: 'circularImage', image: record.photo, label: record.name})
        };

        pushNode(user);

        friends.forEach(v => pushNode(v));

        let edges = friends.map(v => ({from: user.id, to: v.id}));

        // create a network
        let container = document.getElementById('network-container');
        let data = {
            nodes: nodes,
            edges: edges
        };
        let options = {
            layout: {
                improvedLayout: false
            },
            physics: {
                solver: 'repulsion'
            }
        };
        network = new Network(container, data, options);

        network.on("doubleClick", e => {
            value = e.nodes[0].toString();
        });
    });
</script>

<style>
    #network-container {
        width: 100%;
        height: 100%;
    }
</style>

<div id="network-container"></div>