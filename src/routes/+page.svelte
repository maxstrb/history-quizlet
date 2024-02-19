<script>
    import Navbar from '$lib/Navbar.svelte'
    import Footer from '$lib/Footer.svelte';
    import { onMount } from 'svelte';

    onMount(() => {
        document.title = 'Vlastní poznávačka pro Anet';
    });
</script>

<Navbar />
<div class="home_hero">
    <h1>Vlastní poznávačka pro Anet</h1>
    <p>S trochou štestí bude i fungovat</p>
    <a href="/general/">
        <div class="btn">Začít</div>
    </a>
</div>
<Footer />

<style>
    .home_hero {
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        text-align: center;
    }
    .home_hero h1 {
        font-size: 60px;
        width: 70%;
    }
    .home_hero p {
        color: #6e7076;
        font-size: 20px;
    }
    .btn {
        background-color: #5333ed;
        padding: 20px 30px;
        margin-top: 40px;
        border-radius: 5px;
        color: #fff;
    }
    @media (max-width: 700px) {
        .home_hero h1 {
            font-size: 40px;
        }
        .home_hero p {
            font-size: 16px;
        }
    }
</style>