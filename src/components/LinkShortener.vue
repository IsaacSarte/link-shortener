<template>
    <div class="link-shortener">
        <h1>Link Shortener</h1>
        <input v-model="originalUrl" placeholder="Enter URL" />
        <button @click="shortenUrl">Shorten</button>
        <div v-if="errorMessage" class="error">{{ errorMessage }}</div>
        <div v-if="shortenedUrl">
            <p>Shortened URL: <a :href="shortenedUrl" target="_blank">{{ shortenedUrl }}</a></p>
        </div>
    </div>
</template>
  
<script>
    import axios from 'axios';
    
    export default {
        data() {
            return {
                originalUrl: '',
                shortenedUrl: null,
                errorMessage: '',
            };
        },

        methods: {
            isValidUrl(url) {
                const pattern = new RegExp(
                    '^(https?:\\/\\/)?' + // protocol
                    '((([a-z\\d]([a-z\\d-]*[a-z\\d])*)\\.)+[a-z]{2,}|' + // domain name
                    '((\\d{1,3}\\.){3}\\d{1,3}))' + // OR ip (v4) address
                    '(\\:\\d+)?(\\/[-a-z\\d%_.~+]*)*' + // port and path
                    '(\\?[;&a-z\\d%_.~+=-]*)?' + // query string
                    '(\\#[-a-z\\d_]*)?$', 'i' // fragment locator
                );

                return !!pattern.test(url);
            },

            async shortenUrl() {
                this.errorMessage = '';

                if (!this.originalUrl) {
                    this.errorMessage = 'Please enter a URL.';
                    return;
                }
        
                if (!this.isValidUrl(this.originalUrl)) {
                    this.errorMessage = 'Please enter a valid URL.';
                    return;
                }
        
                try {
                    const response = await axios.post(
                        'https://api-ssl.bitly.com/v4/shorten',
                        {
                            long_url: this.originalUrl,
                        },
                        {
                            headers: {
                                Authorization: `Bearer 804988f99ec4e3d5a43a80023b8588cbed103328`, // Replace with your Bitly access token
                                'Content-Type': 'application/json',
                            },
                        }
                    );
        
                    this.shortenedUrl = response.data.link;
                } catch (error) {
                    this.errorMessage = 'An error occurred while shortening the URL.';
                    console.error(error);
                }
            },
        },
    };
</script>
  
<style scoped>
    .link-shortener {
        max-width: 500px;
        margin: 0 auto;
        text-align: center;
        padding: 50px;
    }
    .error {
        color: red;
    }
</style>