<script>
    import Quote from '#lib/components/Quote.svelte';

    let myQuotes = [
        { id:"1", author: "Paul Opitz", text: "Don't be Captain Obvious!" },
        { id:"2", author: "Gandalf", text: "You shall not pass!" },
        { id:"3", author: "Bilbo Baggins", text: "Not all those who wander are lost." },
        { id:"4", author: "Eminem", text: "Hi, my name is, what? My name is, who? My name is, chka-chka, Slim Shady." },
        { id:"5", author: "Shkodra Elektronike", text: "Njerëzit e mirë e njerëzit pa emër, kërcejnë valle n'shpirt. Jarnane ti toka ime, ku kam lindë s'do të harroj." }
    ]

    function dragstart (event) {
        event.dataTransfer.setData("text/plain",event.target.innerText);
    };

    function dragover (event) {
        event.preventDefault();
        event.currentTarget.classList.add("bg-green-200");
    };

    function drop (event) {
        event.preventDefault();
        let id = event.dataTransfer.getData("text/plain");
        const dragegedItem = document.getElementById(id);
        console.log("Dropped data:", dragegedItem);
        event.currentTarget.appendChild(dragegedItem);
        event.currentTarget.classList.remove("bg-green-200");
    };
</script>
 
<h1 class="text-center text-4xl m-6">Data Exchange</h1>
 
<main class="flex">
 
    <div class="flex flex-col gap-4 p-4 bg-red-100 w-1/2" ondragover={dragover} ondrop={drop}>

        {#each myQuotes as quote}
            <div draggable="true" ondragstart={dragstart} class="cursor-grab">
                <Quote author={quote.author}>
                {quote.text}
                </Quote>
            </div>
        {/each}
    </div>
 
    <div class="flex flex-col gap-4 p-4 bg-green-100 w-1/2">
 
    </div>
 
  
 
</main>