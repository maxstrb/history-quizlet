<script>
    import Navbar from '$lib/Navbar.svelte'
    import Footer from '$lib/Footer.svelte';
    import { writable } from "@macfja/svelte-persistent-store";
    import '$lib/global.css';
    import { onMount } from 'svelte';

    onMount(() => {
        document.title = 'Anetin quiz';
    });
    
    function random_item(items) {
        return items[Math.floor(Math.random() * items.length)];
    }

    function remove_item(items, item){
        return items.filter(i => i !== item);
    }

    const quiz_questions = [
        ["Jean Reno \nNǎvštěvníci, Tučnák", "cultureFaces/image1.png"],
        ["Alain Delon \nZoro, Černý tulipán", "cultureFaces/image2.png"],
        ["Jean Marais \nHrbáč, Železná maska", "cultureFaces/image3.png"],
        ["Brigitte Bardot \nfrancouzská herečka", "cultureFaces/image4.png"],
        ["Jean Paul Belmondo „Bepe“ \nVeselé Velikonoce", "cultureFaces/image5.png"],
        ["Catherine Deneuve \nfrancouzská herečka", "cultureFaces/image6.png"],
        ["Michèle Mercierová \nAngelika, Paní Hamiltonová", "cultureFaces/image7.png"],
        ["Yves Montand \nPošetilost mocných", "cultureFaces/image8.png"],
        ["Lino Ventura \nBídníci", "cultureFaces/image9.png"],
        ["Yves Rénier \nKomisař Moulin", "cultureFaces/image10.png"],
        ["Jean Gabin \nkomisař Maigret", "cultureFaces/image11.png"],
        ["Lois de Funès \nFantomas, Oskar", "cultureFaces/image12.png"],
        ["Gérard Depardieu \nfrancouzský herec", "cultureFaces/image13.png"],
        ["Agnetha Fältskog \nABBA", "cultureFaces/image14.png"],
        ["Björn Ulvaeus \nABBA", "cultureFaces/image15.png"],
        ["Benny Andersson \nABBA", "cultureFaces/image16.png"],
        ["Anni-Fryd Lyngstadová \nABBA", "cultureFaces/image17.png"],
        ["Claudia Cardinale \nitalská herečka", "cultureFaces/image18.png"],
        ["Mireille Mathieu \nfrancouzská šansoniérka", "cultureFaces/image19.png"],
        ["Marlene Dietrich \nNěmecká herečka", "cultureFaces/image20.png"],
        ["Elton John \nBritský zpěvák", "cultureFaces/image21.png"],
        ["Sean Connery \nJméno růže", "cultureFaces/image22.png"],
        ["Sophia Loren \nItalská herečka", "cultureFaces/image23.png"],
        ["Anthony Hopkins \nZorro: Tajemná tvář", "cultureFaces/image24.png"],
        ["Roberto Benigni \nitalský herec a režisér", "cultureFaces/image25.png"],
        ["Luciano Pavarotti \noperní pěvec", "cultureFaces/image26.png"],
        ["Michele Placido \nitalský herec a režisér", "cultureFaces/image27.png"],
        ["Plácido Domingo \nšpaňelský operní pěvec", "cultureFaces/image28.png"],
        ["José Carreras \nšpanělský operní pěvec", "cultureFaces/image29.png"],
        ["Montserrat Caballé \nšpanělská operní pěvkyně", "cultureFaces/image30.png"],
        ["Hugh Grant \nLáska nebeská - premiér", "cultureFaces/image31.png"],
        ["Roger Moore \njeden z Bondů", "cultureFaces/image32.png"],
        ["Timothy Dalton \njeden z Bondů", "cultureFaces/image33.png"],
        ["Pierce Brosnan \nirský herec, Bond, Mamma Mia", "cultureFaces/image34.png"],
        ["Stellan Skarsgård \nšvédský herec, Mamma Mia - Bill", "cultureFaces/image35.png"],
        ["Colin Firth \nKrálova řeč, Láska nebeská", "cultureFaces/image36.png"],
        ["Alfréd Molina \nbritsko-americký herec", "cultureFaces/image37.png"],
        ["Herbert Lom \ninspektor Dreyfus, Cornel", "cultureFaces/image38.png"],
        ["Peter Sellers \nRůžový panter", "cultureFaces/image39.png"],
        ["Robert Hossein \nGeoffrey de Peyrac", "cultureFaces/image40.png"],
        ["Arnold Schwarzeneger \nsportovec, herec", "cultureFaces/image41.png"],
        ["Pierre Richard \nHořčice mi stoupá do nosu", "cultureFaces/image42.png"],
        ["Falco \nJohann Hölzel - rakouský zpěvák", "cultureFaces/image43.png"],
        ["Claude Gensac \nfilmová manželka Funése", "cultureFaces/image44.png"],
        ["Audrey Hepburn \nRobin a Mariana, My Fair Lady", "cultureFaces/image45.png"],
        ["Diana Frances Spencer \nPrincezna Walesu", "cultureFaces/image46.png"],
        ["Dodi al-Fayed \npřítel lady Diany", "cultureFaces/image47.png"],
        ["Pedro Almodóvar \nšpanělský režisér", "cultureFaces/image48.png"],
        ["Lars von Trier \ndánský režisér", "cultureFaces/image49.png"],
        ["Jean-Luc Godard \nfrancouzský režisér", "cultureFaces/image50.png"],
        ["Luc Besson \nfrancouzský režisér", "cultureFaces/image51.png"],
        ["Emir Kusturica \nsrbský režisér a hudebník", "cultureFaces/image52.png"],
        ["Ingmar Bergman \nšvédský režisér", "cultureFaces/image53.png"],
        ["Marie Versini \nNšo-či", "cultureFaces/image54.png"],




    ];
    
    // Loading stored data
    let remaining_questions_indexes = writable("remaining_questions_indexes_culture", [...quiz_questions.keys()]);
    let incorrectly_answered_indexes = writable("incorrectly_answered_indexes_culture", []);
    let current_question = writable("current_question_culture", -1);
    let number_of_done_questions = writable("number_of_done_questions_culture", -1);
    let number_of_starting_questions = 0;
    
    if ($number_of_done_questions === -1 || $number_of_done_questions === null || $number_of_done_questions === undefined || $number_of_done_questions > quiz_questions.length || typeof $number_of_done_questions != "number"){
        $number_of_done_questions = 0;
    }
    
    if ($incorrectly_answered_indexes.length > quiz_questions.length){
        $remaining_questions_indexes = [...quiz_questions.keys()];
        $incorrectly_answered_indexes = [];
    } else if ($remaining_questions_indexes.length > quiz_questions.length || $remaining_questions_indexes.length === 0){
        $remaining_questions_indexes = [...quiz_questions.keys()];
        $incorrectly_answered_indexes = [];
        number_of_starting_questions = quiz_questions.length;
    } else{
        number_of_starting_questions = $remaining_questions_indexes.length + $number_of_done_questions;
    }

    if ($current_question === -1 || $current_question === null || $current_question === undefined || $current_question >= quiz_questions.length || typeof $current_question != "number"){
        $current_question = random_item($remaining_questions_indexes);
    }
    // End of loading stored data

    // Loading images and names
    let current_name = quiz_questions[$current_question][0];
    let current_image = quiz_questions[$current_question][1];
    
    let name_split = current_name.split("\n");
    let c_name = name_split[0];
    let c_who = name_split[1];

    let name_revealed = false;
    
    $: {
        current_name = quiz_questions[$current_question][0];
        name_split = current_name.split("\n");
        c_name = name_split[0];
        c_who = name_split[1];

        current_image = quiz_questions[$current_question][1];
    }
    // End of loading images and names
    
    // Button functions
    function reset_quiz(){
        name_revealed = false;
        load_defaults();
    }
    
    function correct_answer(){
        $remaining_questions_indexes = remove_item($remaining_questions_indexes, $current_question);
        load_next_question();
        name_revealed = false;
    }
    
    function wrong_answer(){
        $remaining_questions_indexes = remove_item($remaining_questions_indexes, $current_question);
        $incorrectly_answered_indexes = [...$incorrectly_answered_indexes, $current_question];
        load_next_question();
        name_revealed = false;
    }

    function button_clicked(){
        name_revealed = true;
    }
    // End of button functions


    function load_defaults(){
        $remaining_questions_indexes = [...quiz_questions.keys()];
        $incorrectly_answered_indexes = [];

        $number_of_done_questions = 0;
        number_of_starting_questions = quiz_questions.length;
        
        $current_question = random_item($remaining_questions_indexes);
    }

    function load_next_question(){
        $number_of_done_questions++;
        if ($remaining_questions_indexes.length === 0){
            if ($incorrectly_answered_indexes.length === 0){
                alert("Gratuluji, všechny otázky jsi zodpověděla správně!");
                load_defaults();
                return;
            } else {
                alert("Zodpověděla jsi všechny otázky, které jsi zodpověděla špatně. Teď se ti zobrazí znovu.");
                $remaining_questions_indexes = $incorrectly_answered_indexes;
                $incorrectly_answered_indexes = [];

                $number_of_done_questions = 0;
                number_of_starting_questions = $remaining_questions_indexes.length;
            }
        }
        $current_question = random_item($remaining_questions_indexes);
    }
</script>

<Navbar />
<div id="container">
    <div id="c">
        <div class="rect" id="a"></div>
        <div class="rect" id="b"></div>

        <button on:click={() => button_clicked()} id="reveal">
            <img src={current_image} alt="Mystery person" id="mystery"/>
        </button>

    </div>

    {#if name_revealed}
        <h1 id="name">
            {c_name}
            {#if c_who}
                <br>
                <span id="c-who">{c_who}</span>
            {/if}
        </h1>

        <div>
            <button class="answer_button" on:click={() => correct_answer()}>Věděla jsem</button>
            <button class="answer_button" on:click={() => wrong_answer()}>Nevěděla jsem</button>
        </div>
    
    {:else}
        <h1 id="who"> <span id="vzdavam">Kdo</span> je to? </h1>
    {/if}
    
    <p id="counter">
        {$number_of_done_questions} / {number_of_starting_questions}
    </p>

    <button id="reset_button" on:click={() => {reset_quiz()}}><img src="icons/Reload.svg" alt="Reload" id="reload"/></button>
</div>
<Footer />

<style>
    #name{
        text-align: center;
        font-size: 8vw;
    }

    #reset_button{
        position: absolute;

        background: none;
        border: none;

        margin: 0;
        padding: 0;

        bottom: 7px;
        right: 7px;

        cursor: pointer;
    }

    .answer_button{
        width: 30vw;
        height: 7vh;

        font-size: 4vw;
        color: white;

        margin: 10px;
        padding: 0;

        border: none;
        border-radius: 5px;

        cursor: pointer;

        background: transparent;

        border: #7a2740 2px solid;
    }

    #reload:hover{
        transform: rotate(360deg);
        transition: 0.5s ease-in-out;
    }

    #reload{
        width: 30px;
        height: 30px;

        transition: 0s;
    }

    #counter {
        font-size: 17px;
        color: #747474;
    }

    #c-who {
        font-size: 6vw;
        color: #747474;
    }

    #vzdavam {
        color: #F02461;
    }

    #who {
        margin: 0;
        padding: 0;

        font-size: 15vw;
    }

    #container {
        margin-top: 45px;
        height: 80vh;

        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-between;

        gap: 20px;
    }

    #reveal {
        margin: 0;
        padding: 0;

        border: none !important;
        background: transparent !important;

        width: 100%;
        height: 100%;
    }
    #mystery {
        margin: 0;
        padding: 0;

        width: 100%;
        height: 100%;

        object-fit: cover;
    }

    #c {
        position: relative;
        justify-content: center;

        width: 75vmin;
        height: 75vmin;
    }

    .rect {
        position: absolute;

        width: 100%;
        height: 100%;
        
        border: 1px solid white;
        padding: 6px;

        pointer-events: none;
    }

    #a {
        bottom: 0px;
        right: 0px;
    }

    @media not (max-width: 550px) {
        #who {
            font-size: 50px;
        }
        #name {
            font-size: 25px;
        }
        #c-who {
            font-size: 20px;
        }
        #c{
            width: 50vmin;
            height: 50vmin;
        }

        .answer_button{
            width: 140px;
            height: 55px;
            font-size: 20px;
        }
    }
</style>