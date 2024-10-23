<script>
    import { onMount } from 'svelte';

    let home = false;
    let general = false;
    let secret = false;
    let culture = false;

    onMount(() => {
        switch (window.location.href.split('/').pop()){
            case 'general':
                general = true;
                break;
            
            case 'secret':
                secret = true;
                break;
            
            case 'culture':
                culture = true;
                break;

            default:
                home = true;
                break;
        }
    });
</script>

<nav>
    <a href="/" id="home" class="{home ? "a-home": "i-home"}"><img src="icons/HomeIcon.svg" alt="Home"></a>
    <div id="links">
        <a href="/general" id="general" class="{general ? "active": "inactive"}">Politika</a>
        <a href="/culture" id="culture" class="{culture ? "active": "inactive"}">Kultura</a>
        <a href="/reminder" id="secret" class="{secret ? "active": "inactive"}">Připomínka</a>
    </div>
</nav>

<style>
    nav {
        display: flex;
        justify-content: space-between;
        flex-direction: row;
        align-items: center;
        padding: 20px;
        gap: 20px;
        background-color: #363040;

        height: auto;
        border-bottom: 1px solid #636363;
    }

    #links {
        display: flex;
        gap: 20px;
    }

    #home{
        mask: url(icons/HomeIcon.svg) no-repeat center;
        mask-type: alpha;
    }

    #home img{
        opacity: 0;
    }

    .i-home{
        background-color: white;
        transition: 0.2s ease-in-out;
    }

    .i-home:hover{
        background-color: #F02461;
    }
    .a-home{
        background-color: #F02461;
    }

    nav a {
        color: white;
        text-decoration: none;
    }

    .active{
        color: #F02461;
    }

    .inactive {
        color: white;

        transition: 0.2s ease-in-out;
    }

    .inactive:hover{
        color: #F02461;
    }
</style>