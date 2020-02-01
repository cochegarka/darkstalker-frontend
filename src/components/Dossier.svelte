<script>
    import Icon from 'svelte-awesome';
    import {check} from 'svelte-awesome/icons';

    export let user;
    export let friendsCount;

    $: name = `${user.first_name || ''} ${user.last_name || ''}`;
    let sex = `${(() => {
        switch (user.sex) {
            case 1:
                return 'female';
            case 2:
                return 'male';
            default:
                return 'not indicated';
        }
    })()}`;

    let occupation = `${(t => {
        if (t === "work") {
            return "working";
        } else if (t === "school" || t === "university") {
            return "studying";
        }
    })(user.occupation.type)}`;
</script>

<style>
    .primary {
        padding: 16px;
        margin: 0 48px;
        text-align: center;
        border-bottom: 2px solid #787878;
    }

    .common {
        padding: 16px;
        margin: 0 48px;
        border-bottom: 2px solid #787878;
    }

    .end {
        padding: 16px;
        margin: 0 48px;
    }

    .primary-image {
        border-radius: 50%;
    }

    h1, h3, h4 {
        margin: 8px;
    }

    li {
        list-style-type: none;
    }
</style>

<div id="container">
    <div class="primary">
        <img src={user.photo_200} alt="user photo" class="primary-image"/>
        <h1>
            {#if user.verified}
                <Icon style="color: black;" data={check} scale="1.5"/>
            {/if}
            {name}
        </h1>
        <h4 style="font-style: italic">{user.status || ''}</h4>
    </div>
    <div class="common">
        <ul>
            <li><b>Sex</b>: {sex}</li>
            <li><b>Birthday</b>: {user.bdate || ''}</li>
        </ul>
        <ul>
            <li><b>City</b>: {user.city.title || ''}</li>
            <li><b>Country</b>: {user.country.title || ''}</li>
        </ul>
        <ul>
            <li>
                <b>Occupation</b>: {occupation} in {user.occupation.name || ''}
            </li>
        </ul>
        <ul>
            <li>
                <b>Graduated in</b> {user.university_name || ''} in {user.graduation || ''}
            </li>
        </ul>
    </div>
    <div class="end">
        <ul>
            <li>
                <b>Followers</b>: {user.followers_count}
            </li>
            <li>
                <b>Visible friends</b>: {friendsCount}
            </li>
        </ul>
    </div>
</div>