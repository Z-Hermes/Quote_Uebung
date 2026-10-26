<script>
    let { author, children } = $props();
    let quote;

    async function copyQuote() {
        const text = `${quote.textContent.trim()}\n— ${author}`;

        try {
            await navigator.clipboard.writeText(text);
        } catch (error) {
            console.error('Could not copy quote:', error);
        }
    }

    async function shareQuote() {
        const text = `${quote.textContent.trim()}\n— ${author}`;

        try {
            if (navigator.share) {
                await navigator.share({
                    title: `Quote by ${author}`,
                    text
                });
            } else {
                await navigator.clipboard.writeText(text);
            }
        } catch (error) {
            if (error.name !== 'AbortError') {
                console.error('Could not share quote:', error);
            }
        }
    }
</script>

<article class="overflow-hidden rounded-2xl border border-slate-200 bg-white shadow-sm transition hover:shadow-md">
    <header class="flex items-center gap-3 border-b border-slate-100 px-5 py-4 sm:px-6">
        <div class="flex h-10 w-10 items-center justify-center rounded-full bg-teal-100 font-bold text-teal-800">
            {author.charAt(0)}
        </div>

        <h2 class="font-semibold text-slate-900">
            {author}
        </h2>
    </header>

    <div class="px-5 py-6 sm:px-6">
        <blockquote
            bind:this={quote}
            class="border-l-4 border-teal-500 pl-4 text-lg leading-relaxed text-slate-700"
        >
            {@render children?.()}
        </blockquote>

        <div class="mt-6 flex flex-wrap gap-3">
            <button
                onclick={copyQuote}
                class="rounded-lg bg-teal-700 px-4 py-2 text-sm font-medium text-white transition hover:bg-teal-800 focus:outline-2 focus:outline-offset-2 focus:outline-teal-700"
            >
                Copy
            </button>

            <button
                onclick={shareQuote}
                class="rounded-lg border border-slate-300 bg-white px-4 py-2 text-sm font-medium text-slate-700 transition hover:border-teal-600 hover:bg-teal-50 hover:text-teal-800 focus:outline-2 focus:outline-offset-2 focus:outline-teal-700"
            >
                Share
            </button>
        </div>
    </div>
</article>