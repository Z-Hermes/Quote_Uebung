<script>
    let { children } = $props();
    let quote;

    async function copyQuote() {
        await navigator.clipboard.writeText(quote.textContent.trim());
    }

    async function shareQuote() {
        await navigator.share({
            title: "Quote",
            text: quote.textContent.trim(),
        });
    }
</script>
 
    <header class="bg-gray-800 text-white p-4">
        {author}says:
    </header>

<blockquote bind:this={quote} class="border-l-4 border-gray-500 pl-4 italic text-gray-600 mb-4">
 
        {@render children?.()}
</blockquote>

<button onclick={shareQuote}>
    Teilen
</button>
<button onclick={copyQuote}>
    Kopieren
</button>