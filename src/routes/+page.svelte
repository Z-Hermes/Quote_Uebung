<script>
    // Import the reusable Quote component.
    // Each quote will use the same design and buttons.
    import Quote from '#lib/components/Quote.svelte';
</script>

<svelte:head>
    <!-- Set the title shown in the browser tab. -->
    <title>Data Exchange | Quotes</title>

    <!-- Describe the page for search engines. -->
    <meta
        name="description"
        content="Five inspirational quotes to copy and share."
    />
</svelte:head>

<!-- Main page container with a light background and spacing. -->
<main class="min-h-screen bg-slate-100 px-4 py-12 text-slate-800 sm:px-6">
    <!-- Keep the content centered and prevent it from becoming too wide. -->
    <div class="mx-auto max-w-3xl">

        <!-- Page heading and short introduction. -->
        <header class="mb-10 text-center">
            <p class="mb-3 text-sm font-semibold uppercase tracking-[0.2em] text-teal-700">
                Words to remember
            </p>

            <h1 class="text-4xl font-bold tracking-tight text-slate-900 sm:text-5xl">
                Data Exchange
            </h1>

            <p class="mt-4 text-base text-slate-600">
                Five quotes worth sharing.
            </p>
        </header>

        <!-- Display all five quotes with consistent spacing. -->
        <section class="space-y-5">

            <!-- First quote. -->
            <Quote author="Abraham Lincoln">
                My dream is of a place and a time where America will once again be seen as the last best hope of earth.
            </Quote>

            <!-- Second quote. -->
            <Quote author="Oscar Wilde">
                Be yourself; everyone else is already taken.
            </Quote>

            <!-- Third quote. -->
            <Quote author="Albert Einstein">
                Imagination is more important than knowledge.
            </Quote>

            <!-- Fourth quote. -->
            <Quote author="Mahatma Gandhi">
                Be the change that you wish to see in the world.
            </Quote>

            <!-- Fifth quote. -->
            <Quote author="Mark Twain">
                The secret of getting ahead is getting started.
            </Quote>

        </section>

        <!-- Small footer at the bottom of the page. -->
        <footer class="mt-10 text-center text-sm text-slate-500">
            Choose a quote and share a little inspiration.
        </footer>

    </div>
</main>