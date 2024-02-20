<script>
    import Navbar from '$lib/Navbar.svelte'
    import Footer from '$lib/Footer.svelte';
    import '$lib/global.css';
    import { onMount } from 'svelte';

    onMount(() => {
        document.title = 'Anetin quiz';
    });
</script>

<Navbar />
<div class="main">
    <div id="text-container">
        <h1 id="text">Jenom taková připomínka že tě</h1>
        <h2 id="anet">miluju</h2>
    </div>
</div>
<Footer />

<style>
    .main {
        margin-top: 7vh;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-around;
        gap: 20px;
        height: 70vh;
    }
    
    #anet {
        color: #F02461;
        font-size: 12vw;
        margin: 0;
        padding: 0;
    }

    #text-container {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        gap: 5px;
    }
    #text {
        font-size: 8vw;
        text-align: center;
        margin: 0;
        padding: 0;
    }

    @media not (max-width: 600px) {
        #text-container {
            gap: 10px;
        }

        #text {
            font-size: 40px;
        }

        #anet {
            font-size: 60px;
        }
    }
</style>