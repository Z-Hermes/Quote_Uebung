<script>

    // Get the author and quote content from the parent component.
    let { author, children } = $props();

    // Reference the HTML element containing the quote.
    let quote;

    // Copy the quote and show a system notification.
    async function copyQuote() {
        const text = `${quote.textContent.trim()}\n— ${author}`;

        await navigator.clipboard.writeText(text);

        // Request permission if it has not been decided yet.
        if (Notification.permission === 'default') {
            await Notification.requestPermission();
        }

        // Show a notification through the operating system.
        if (Notification.permission === 'granted') {
            new Notification('Quote copied!', {
                body: `The quote by ${author} was copied.`
            });
        }
    }

    // Share the quote using the browser's sharing feature.
    async function shareQuote() {
        const text = `${quote.textContent.trim()}\n— ${author}`;

        if (navigator.share) {
            await navigator.share({
                title: `Quote by ${author}`,
                text
            });
        } else {
            // Copy the quote if sharing is not supported.
            await copyQuote();
        }
    }
</script>

<!-- Card containing the author, quote and action buttons. -->
<article
    class="overflow-hidden rounded-2xl border border-slate-200 bg-white shadow-sm transition hover:shadow-md"
>
    <!-- Header displays the author's initial and name. -->
    <header class="flex items-center gap-3 border-b border-slate-100 px-5 py-4 sm:px-6">

        <!-- Show the first letter of the author's name in a circle. -->
        <div
            class="flex h-10 w-10 items-center justify-center rounded-full bg-teal-100 font-bold text-teal-800"
        >
            {author.charAt(0)}
        </div>

        <!-- Display the author's name passed from the parent. -->
        <h2 class="font-semibold text-slate-900">
            {author}
        </h2>
    </header>

    <!-- Main content area of the quote card. -->
    <div class="px-5 py-6 sm:px-6">

        <!--
            bind:this stores a reference to this HTML element.
            quote.textContent can then retrieve the displayed text
            when the user clicks Copy or Share.
        -->
        <blockquote
            bind:this={quote}
            class="border-l-4 border-teal-500 pl-4 text-lg leading-relaxed text-slate-700"
        >
            <!-- Render the quote content provided by the parent. -->
            {@render children?.()}
        </blockquote>

        <!-- Buttons for copying and sharing the quote. -->
        <div class="mt-6 flex flex-wrap gap-3">

            <!-- Copy button writes the quote and author to the clipboard. -->
            <button
                onclick={copyQuote}
                class="rounded-lg bg-teal-700 px-4 py-2 text-sm font-medium text-white transition hover:bg-teal-800 focus:outline-2 focus:outline-offset-2 focus:outline-teal-700"
            >
                Copy
            </button>

            <!-- Share button opens the native sharing menu when available. -->
            <button
                onclick={shareQuote}
                class="rounded-lg border border-slate-300 bg-white px-4 py-2 text-sm font-medium text-slate-700 transition hover:border-teal-600 hover:bg-teal-50 hover:text-teal-800 focus:outline-2 focus:outline-offset-2 focus:outline-teal-700"
            >
                Share
            </button>

        </div>
    </div>
</article>