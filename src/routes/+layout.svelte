<script>
    // Import the global stylesheet containing Tailwind CSS.
    import './layout.css';

    // Import the existing favicon for the browser tab.
    import favicon from '#lib/assets/favicon.svg';

    // Receive the page content from the current route.
    let { children } = $props();
</script>

<svelte:head>
    <!-- Set the favicon displayed in the browser tab. -->
    <link rel="icon" href={favicon} />
</svelte:head>

<!-- Render the current page inside the shared layout. -->
{@render children()}