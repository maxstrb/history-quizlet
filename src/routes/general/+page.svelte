<script>
    import { onMount } from 'svelte';
    import Navbar from '$lib/Navbar.svelte'
    import Footer from '$lib/Footer.svelte';
    import { writable } from "@macfja/svelte-persistent-store";
    
    onMount(() => {
        document.title = 'Vlastní poznávačka pro Anet';
    });
    
    function random_item(items) {
        return items[Math.floor(Math.random() * items.length)];
    }

    function remove_item(items, item){
        return items.filter(i => i !== item);
    }

    const quiz_questions = [
        ["Jerzy Popieluszko (polský mučedník)", "generalFaces/Jerzy Popieluszko (polský mučedník).png"],
        ["Lech Wałęsa (polský prezident)", "generalFaces/Lech Wałęsa (polský prezident).png"],
        ["Nicolae Ceausescu (rumunský diktátor)", "generalFaces/Nicolae Ceausescu (rumunský diktátor).png"],
        ["Slobodan Milošević (srbský prezident)", "generalFaces/Slobodan Milošević (srbský prezident).png"],
        ["Franjo Tuđman (chorvatský prezident)", "generalFaces/Franjo Tuđman (chorvatský prezident).png"],
        ["Slobodan Praljak (chorvatský generál)", "generalFaces/Slobodan Praljak (chorvatský generál).png"],
        ["Ante Gotovina (chorvatský generál)", "generalFaces/Ante Gotovina (chorvatský generál).jpg"],
        ["Stjepan Mesić (chorvatský politik a voják)", "generalFaces/Stjepan Mesić (chorvatský politik a voják).png"],
        ["Ratko Mladić (srbský generál)", "generalFaces/Ratko Mladić (srbský generál).png"],
        ["Milan Kučan (slovinský prezident)", "generalFaces/Milan Kučan (slovinský prezident).png"],
        ["Alija Izetbegović (prezident Bosny)", "generalFaces/Alija Izetbegović (prezident Bosny).png"],
        ["Petar Mladenov (bulharský prezident)", "generalFaces/Petar Mladenov (bulharský prezident).png"],
        ["Želju Želev (bulharský prezident, spisovatel)", "generalFaces/Želju Želev (bulharský prezident, spisovatel).png"],
        ["Leonid Kravčuk (1. ukrajinský prezident)", "generalFaces/Leonid Kravčuk (1. ukrajinský prezident).png"],
        ["Leonid Kučma (2. ukrajinský prezident)", "generalFaces/Leonid Kučma (2. ukrajinský prezident).png"],
        ["Gerald Ford (38. prezident USA)", "generalFaces/Gerald Ford (38. prezident USA).png"],
        ["James Carter (39. prezident USA)", "generalFaces/James Carter (39. prezident USA).png"],
        ["Ronald Reagan (40. prezident USA)", "generalFaces/Ronald Reagan (40. prezident USA).png"],
        ["George H.W. Bush (41. prezident USA)", "generalFaces/George H.W. Bush (41. prezident USA).png"],
        ["Bill Clinton (42. prezident USA)", "generalFaces/Bill Clinton (42. prezident USA).png"],
        ["Monika Lewinská", "generalFaces/Monika Lewinská.png"],
        ["Kurt Waldheim (4. generální tajemník OSN)", "generalFaces/Kurt Waldheim (4. generální tajemník OSN).png"],
        ["Javier Pérez de Cuéllar (5. gen. tajemník OSN)", "generalFaces/Javier Pérez de Cuéllar (5. gen. tajemník OSN).png"],
        ["Butrus Butrus-Ghálí (6. gen. tajemník OSN)", "generalFaces/Butrus Butrus-Ghálí (6. gen. tajemník OSN).png"],
        ["Kofi Annan (7. generální tajemník OSN)", "generalFaces/Kofi Annan (7. generální tajemník OSN).png"],
        ["Arpád Göncz (maďarský prezident)", "generalFaces/Arpád Göncz (maďarský prezident).png"],
        ["Viktor Orbán", "generalFaces/Viktor Orbán.png"],
        ["Erich Honecker (nástupce Ulbrichta)", "generalFaces/Erich Honecker (nástupce Ulbrichta).png"],
        ["Hans Dietrich-Genscher (ministr zahraničí Německa, předseda OBSE)", "generalFaces/Hans Dietrich-Genscher (ministr zahraničí Německa, předseda OBSE).png"],
        ["Helmut Kohl (kancléř SRN)", "generalFaces/Helmut Kohl (kancléř SRN).png"],
        ["Willy Brandt (kancléř SRN a předseda SPD)", "generalFaces/Willy Brandt (kancléř SRN a předseda SPD).png"],
        ["François Mitterrand (francouzský prezident)", "generalFaces/François Mitterrand (francouzský prezident).png"],
        ["Jacques Chiraq (francouzský prezident)", "generalFaces/Jacques Chiraq (francouzský prezident).png"],
        ["Eduard Ševardnadze (ministr zahraničí SSSR)", "generalFaces/Eduard Ševardnadze (ministr zahraničí SSSR).png"],
        ["Olof Palme (švédský premiér)", "generalFaces/Olof Palme (švédský premiér).png"],
        ["Günter Schabowski (pád Berlínské zdi)", "generalFaces/Günter Schabowski (pád Berlínské zdi).png"],
        ["Tony Blair (britský premiér)", "generalFaces/Tony Blair (britský premiér).png"],
        ["Margaret Thatcherová (britská premiérka)", "generalFaces/Margaret Thatcherová (britská premiérka).png"],
        ["John Major (britský premiér)", "generalFaces/John Major (britský premiér).png"],
        ["James Callaghan (britský premiér)", "generalFaces/James Callaghan (britský premiér).png"],
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
    
    let name_revealed = false;
    
    $: {
        current_name = quiz_questions[$current_question][0];
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
    <button id="reset_button" on:click={() => {reset_quiz()}}>Reset</button>
    <button id="guess_button" on:click={ () => {button_clicked()}}>

        <img alt="" id="guess_img" src="{current_image}">
    </button>

    {#if name_revealed}
        <h1>{current_name}</h1>
        <div>
            <button class="answer_button" on:click={() => correct_answer()}>Věděla jsem</button>
            <button class="answer_button" on:click={() => wrong_answer()}>Nevěděla jsem</button>
        </div>

    {:else}
        <h1> Kdo je to? </h1>
    {/if}

    <p>
        {$number_of_done_questions} / {number_of_starting_questions}
    </p>

</div>
<Footer />

<style>
    #reset_button {
        width: 9vh;
        height: 40px;
        background-color: #5333ed;
        padding: 5px;
        margin-top: 10px;
        border-radius: 5px;
        color: #fff;
    }

    #container {
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        text-align: center;
        height: 72vh;
    }

    #guess_img {
        width: 100%;
        max-width: 300px;
        border-radius: 10px;
    }
    
    #guess_button {
        max-width: 80%;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        text-align: center;
        margin-top: 20px;
        height: 69vh;
        overflow: hidden;
    }

    .answer_button {
        background-color: #5333ed;
        padding: 10px 20px;
        margin-top: 10px;
        border-radius: 5px;
        color: #fff;
    }
</style>