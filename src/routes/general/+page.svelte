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
        ["Jerzy Popieluszko \npolský mučedník", "generalFaces/Jerzy Popieluszko (polský mučedník).png"],
        ["Lech Wałęsa \npolský prezident", "generalFaces/Lech Wałęsa (polský prezident).png"],
        ["Nicolae Ceausescu \nrumunský diktátor", "generalFaces/Nicolae Ceausescu (rumunský diktátor).png"],
        ["Slobodan Milošević \nsrbský prezident", "generalFaces/Slobodan Milošević (srbský prezident).png"],
        ["Franjo Tuđman \nchorvatský prezident", "generalFaces/Franjo Tuđman (chorvatský prezident).png"],
        ["Slobodan Praljak \nchorvatský generál", "generalFaces/Slobodan Praljak (chorvatský generál).png"],
        ["Ante Gotovina \nchorvatský generál", "generalFaces/Ante Gotovina (chorvatský generál).jpg"],
        ["Stjepan Mesić \nchorvatský politik a voják", "generalFaces/Stjepan Mesić (chorvatský politik a voják).png"],
        ["Ratko Mladić \nsrbský generál", "generalFaces/Ratko Mladić (srbský generál).png"],
        ["Milan Kučan \nslovinský prezident", "generalFaces/Milan Kučan (slovinský prezident).png"],
        ["Alija Izetbegović \nprezident Bosny", "generalFaces/Alija Izetbegović (prezident Bosny).png"],
        ["Petar Mladenov \nbulharský prezident", "generalFaces/Petar Mladenov (bulharský prezident).png"],
        ["Želju Želev \nbulharský prezident, spisovatel", "generalFaces/Želju Želev (bulharský prezident, spisovatel).png"],
        ["Leonid Kravčuk \n1. ukrajinský prezident", "generalFaces/Leonid Kravčuk (1. ukrajinský prezident).png"],
        ["Leonid Kučma \n2. ukrajinský prezident", "generalFaces/Leonid Kučma (2. ukrajinský prezident).png"],
        ["Gerald Ford \n38. prezident USA", "generalFaces/Gerald Ford (38. prezident USA).png"],
        ["James Carter \n39. prezident USA", "generalFaces/James Carter (39. prezident USA).png"],
        ["Ronald Reagan \n40. prezident USA", "generalFaces/Ronald Reagan (40. prezident USA).png"],
        ["George H.W. Bush \n41. prezident USA", "generalFaces/George H.W. Bush (41. prezident USA).png"],
        ["Bill Clinton \n42. prezident USA", "generalFaces/Bill Clinton (42. prezident USA).png"],
        ["Monika Lewinská", "generalFaces/Monika Lewinská.png"],
        ["Kurt Waldheim \n4. generální tajemník OSN", "generalFaces/Kurt Waldheim (4. generální tajemník OSN).png"],
        ["Javier Pérez de Cuéllar \n5. gen. tajemník OSN", "generalFaces/Javier Pérez de Cuéllar (5. gen. tajemník OSN).png"],
        ["Butrus Butrus-Ghálí \n6. gen. tajemník OSN", "generalFaces/Butrus Butrus-Ghálí (6. gen. tajemník OSN).png"],
        ["Kofi Annan \n7. generální tajemník OSN", "generalFaces/Kofi Annan (7. generální tajemník OSN).png"],
        ["Arpád Göncz \nmaďarský prezident", "generalFaces/Arpád Göncz (maďarský prezident).png"],
        ["Viktor Orbán", "generalFaces/Viktor Orbán.png"],
        ["Erich Honecker \nnástupce Ulbrichta", "generalFaces/Erich Honecker (nástupce Ulbrichta).png"],
        ["Hans Dietrich-Genscher \nministr zahraničí Německa, předseda OBSE", "generalFaces/Hans Dietrich-Genscher (ministr zahraničí Německa, předseda OBSE).png"],
        ["Helmut Kohl \nkancléř SRN", "generalFaces/Helmut Kohl (kancléř SRN).png"],
        ["Willy Brandt \nkancléř SRN a předseda SPD", "generalFaces/Willy Brandt (kancléř SRN a předseda SPD).png"],
        ["François Mitterrand \nfrancouzský prezident", "generalFaces/François Mitterrand (francouzský prezident).png"],
        ["Jacques Chiraq \nfrancouzský prezident", "generalFaces/Jacques Chiraq (francouzský prezident).png"],
        ["Eduard Ševardnadze \nministr zahraničí SSSR", "generalFaces/Eduard Ševardnadze (ministr zahraničí SSSR).png"],
        ["Olof Palme \nšvédský premiér", "generalFaces/Olof Palme (švédský premiér).png"],
        ["Günter Schabowski \npád Berlínské zdi", "generalFaces/Günter Schabowski (pád Berlínské zdi).png"],
        ["Tony Blair \nbritský premiér", "generalFaces/Tony Blair (britský premiér).png"],
        ["Margaret Thatcherová \nbritská premiérka", "generalFaces/Margaret Thatcherová (britská premiérka).png"],
        ["John Major \nbritský premiér", "generalFaces/John Major (britský premiér).png"],
        ["James Callaghan \nbritský premiér", "generalFaces/James Callaghan (britský premiér).png"],
    ];
    
    // Loading stored data
    let remaining_questions_indexes = writable("remaining_questions_indexes", [...quiz_questions.keys()]);
    let incorrectly_answered_indexes = writable("incorrectly_answered_indexes", []);
    let current_question = writable("current_question", -1);
    let number_of_done_questions = writable("number_of_done_questions", -1);
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