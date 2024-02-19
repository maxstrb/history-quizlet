<nav>
    <div class="nav-links">
        <a href="/">Home</a>
        <a href="/general">Obecná Poznávačka</a>
    </div>
</nav>

<style>
    nav {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 20px 40px;
        background-color: #5333ed;
    }

    .nav-links a {
        color: #fff;
        text-decoration: none;
        margin-right: 20px;
    }

    .nav-links a:hover {
        text-decoration: underline;
    }

    @media (max-width: 700px) {
        .nav-links {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }
    }
</style>