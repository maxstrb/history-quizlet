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
        <h1 id="text">Vlastní poznávačka pro </h1>
        <h2 id="anet">Anet</h2>
    </div>
    <a href="/general" id="start"><p id="start-text">START</p></a>
    <p id="message">Hodně štěstí</p>
</div>
<Footer />

<style>
    .main {
        margin-top: 7vh;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-between;
        gap: 20px;
        height: 70vh;
    }

    #start {
        width: 30vh;
        height: 30vh;
        margin: 0;
        padding: 0;

        border: 0.9vh solid #747474;
        border-radius: 100%;

        display: flex;
        align-items: center;
        justify-content: center;
    }

    #start-text {
        margin: 0;
        padding: 0;
        
        position: relative;

        color: #F02461;
        text-decoration: none;

        text-align: center;
        font-weight: bold;
        font-size: 8vh;
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
    #message {
        font-size: 6vw;
        text-align: center;
        margin: 0;
        padding: 0;
        color: #747474;
    }

    @media not (max-width: 600px) {
        #start {
            width: 150px;
            height: 150px;
            border-width: 5px;
        }
        #start-text {
            font-size: 40px;
        }

        #text-container {
            gap: 10px;
        }

        #text {
            font-size: 40px;
        }

        #anet {
            font-size: 60px;
        }

        #message {
            font-size: 30px;
        }
    }
</style>