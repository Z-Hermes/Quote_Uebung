<script>
    import { getDistance } from 'geolib';
    import Quote from '#lib/components/Quote.svelte';

    // Quotes start in the left column. Drag them between the two columns.
    let leftQuotes = $state([
        { id: '1', author: 'Paul Opitz', text: "Don't be Captain Obvious!" },
        { id: '2', author: 'Gandalf', text: 'You shall not pass!' },
        { id: '3', author: 'Bilbo Baggins', text: 'Not all those who wander are lost.' },
        { id: '4', author: 'Eminem', text: 'Hi, my name is, what? My name is, who? My name is, chka-chka, Slim Shady.' },
        { id: '5', author: 'Shkodra Elektronike', text: "Njerëzit e mirë e njerëzit pa emër, kërcejnë valle n'shpirt. Jarnane ti toka ime, ku kam lindë s'do të harroj." }
    ]);
    let rightQuotes = $state([]);

    let locationMessage = $state('');
    let secretFound = $state(false);
    let watchId;

    // Remember which quote is being dragged.
    function dragStart(event, quoteId) {
        event.dataTransfer.setData('text/plain', quoteId);
        event.dataTransfer.effectAllowed = 'move';
    }

    // Allow a quote to be dropped into either column.
    function dragOver(event) {
        event.preventDefault();
        event.dataTransfer.dropEffect = 'move';
    }

    // Move the quote between the columns without duplicating it.
    function dropQuote(event, destination) {
        event.preventDefault();
        const quoteId = event.dataTransfer.getData('text/plain');
        if (!quoteId) return;

        const source = destination === 'left' ? rightQuotes : leftQuotes;
        const target = destination === 'left' ? leftQuotes : rightQuotes;
        const index = source.findIndex((quote) => quote.id === quoteId);

        if (index !== -1) {
            const [quote] = source.splice(index, 1);
            target.push(quote);
        }
    }

    // Watch the device's location and compare it with the secret coordinates.
    // A 100-metre radius allows for normal GPS inaccuracies.
    function checkLocation(position) {
        const currentPosition = {
            latitude: position.coords.latitude,
            longitude: position.coords.longitude
        };
        const secretLocation = {
            latitude: 42.05910523134263,
            longitude: 19.517268301553923
        };
        const distance = getDistance(currentPosition, secretLocation);

        if (distance <= 100) {
            secretFound = true;
            locationMessage = 'You found the secret location!';
            if (watchId !== undefined) {
                navigator.geolocation.clearWatch(watchId);
                watchId = undefined;
            }
        } else {
            locationMessage = `Location checked. You are approximately ${distance} metres away.`;
        }
    }

    // Start location tracking only after the user presses the button.
    function findSecretQuote() {
        if (!('geolocation' in navigator)) {
            locationMessage = 'Geolocation is not supported by this browser.';
            return;
        }

        if (watchId !== undefined) {
            navigator.geolocation.clearWatch(watchId);
        }

        locationMessage = 'Waiting for location permission...';
        watchId = navigator.geolocation.watchPosition(
            checkLocation,
            () => {
                locationMessage = 'Location access failed. Allow location access and try again.';
            },
            { enableHighAccuracy: true, maximumAge: 10000, timeout: 15000 }
        );
    }
</script>

<svelte:head>
    <title>Data Exchange | Quotes</title>
    <meta name="description" content="Move, copy and share your favourite quotes." />
</svelte:head>

<main class="min-h-screen bg-slate-100 px-4 py-8 text-slate-800 sm:px-6">
    <div class="mx-auto max-w-7xl">
        <header class="mb-8 text-center">
            <p class="mb-2 text-sm font-semibold uppercase tracking-[0.2em] text-sky-700">Words to remember</p>
            <h1 class="text-4xl font-bold tracking-tight text-slate-900 sm:text-5xl">Data Exchange</h1>
            <p class="mt-3 text-slate-600">Drag quotes between the two columns. Copy or share them anytime.</p>
        </header>

        <section class="grid grid-cols-1 gap-5 md:grid-cols-2" aria-label="Quote columns">
            <!-- Left drop zone: quotes can be dropped here from either column. -->
            <div
                class="min-h-64 rounded-2xl border border-rose-200 bg-rose-50 p-4 sm:p-5"
                ondragover={dragOver}
                ondrop={(event) => dropQuote(event, 'left')}
            >
                <h2 class="mb-4 flex items-center justify-between font-semibold text-slate-800">
                    Left column <span class="rounded-full bg-white px-3 py-1 text-xs text-slate-600">{leftQuotes.length} quotes</span>
                </h2>
                <div class="space-y-4">
                    {#each leftQuotes as quote (quote.id)}
                        <div draggable="true" ondragstart={(event) => dragStart(event, quote.id)} class="cursor-grab active:cursor-grabbing">
                            <Quote author={quote.author}>{quote.text}</Quote>
                        </div>
                    {/each}
                </div>
                {#if leftQuotes.length === 0}
                    <p class="rounded-xl border border-dashed border-rose-300 p-6 text-center text-sm text-slate-500">Drop quotes here</p>
                {/if}
            </div>

            <!-- Right drop zone: moving a quote back works the same way. -->
            <div
                class="min-h-64 rounded-2xl border border-emerald-200 bg-emerald-50 p-4 sm:p-5"
                ondragover={dragOver}
                ondrop={(event) => dropQuote(event, 'right')}
            >
                <h2 class="mb-4 flex items-center justify-between font-semibold text-slate-800">
                    Right column <span class="rounded-full bg-white px-3 py-1 text-xs text-slate-600">{rightQuotes.length} quotes</span>
                </h2>
                <div class="space-y-4">
                    {#each rightQuotes as quote (quote.id)}
                        <div draggable="true" ondragstart={(event) => dragStart(event, quote.id)} class="cursor-grab active:cursor-grabbing">
                            <Quote author={quote.author}>{quote.text}</Quote>
                        </div>
                    {/each}
                </div>
                {#if rightQuotes.length === 0}
                    <p class="rounded-xl border border-dashed border-emerald-300 p-6 text-center text-sm text-slate-500">Drop quotes here</p>
                {/if}
            </div>
        </section>

        <!-- Easter egg: location access starts only after this button is clicked. -->
        <section class="mx-auto mt-8 max-w-2xl rounded-2xl border border-slate-200 bg-white p-5 text-center shadow-sm sm:p-6">
            <h2 class="text-lg font-semibold text-slate-900">Find the secret quote</h2>
            <p class="mt-2 text-sm text-slate-600">Allow location access and visit the hidden coordinates to unlock it.</p>
            <button
                type="button"
                onclick={findSecretQuote}
                class="mt-4 rounded-lg bg-slate-900 px-5 py-2.5 text-sm font-semibold text-white transition hover:bg-slate-700 focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-slate-900"
            >
                Check my location
            </button>
            {#if locationMessage}
                <p role="status" class="mt-3 text-sm text-slate-600">{locationMessage}</p>
            {/if}
            {#if secretFound}
                <blockquote class="mt-5 rounded-xl border-l-4 border-amber-400 bg-amber-50 p-5 text-left">
                    <p class="text-xs font-semibold uppercase tracking-wider text-amber-800">Secret quote unlocked</p>
                    <p class="mt-2 text-xl italic text-slate-800">“The world is full of magic things, patiently waiting for our senses to grow sharper.”</p>
                    <footer class="mt-2 text-sm text-slate-600">— W. B. Yeats</footer>
                </blockquote>
            {/if}
        </section>
    </div>
</main>
