<template>
    <div id="app">
        <!-- Replace with header component -->
         <header class="header">
            <span>Link-ty</span>

            <div class="right-header">
                <span>About</span>
                <a href="https://github.com/isaacsarte/link-shortener" target="_blank">Github</a>
            </div>
         </header>

        <LinkShortener />
    </div>
</template>
  
<script>
    import LinkShortener from './components/LinkShortener.vue';
    
    export default {
        name: 'App',
        components: {
            LinkShortener,
        },
    };
</script>