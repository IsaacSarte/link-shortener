<template>
    <div class="link-shortener">
        <p class="title">Link-ty</p>
        <p class="description">Make your links shorter!</p>

        <div class="input-container">
            <input v-model="originalUrl" placeholder="Enter URL" />
            <button @click="shortenUrl">Short it!</button>
        </div>
        
        <div v-if="errorMessage" class="error">{{ errorMessage }}</div>
        <div v-if="shortenedUrl" class="shortened-url">
            <a :href="shortenedUrl" target="_blank">{{ shortenedUrl }}</a>
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
                                Authorization: `Bearer 804988f99ec4e3d5a43a80023b8588cbed103328`,
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
    }

    .title {
        font-size: 4rem;
        font-weight: 700;
        margin-bottom: -0.0005rem;
    }

    .description {
        font-size: 1.5rem;
        font-weight: 600;
        opacity: 0.75;
    }

    .input-container {
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .link-shortener input {
        width: 100%;
        font-size: 1.5rem;
        padding: 0.375rem 0.5rem 0.35rem 0.5rem;
        outline: none;
        border-top-left-radius: 5px;
        border-bottom-left-radius: 5px;
        border-right: none;
    }

    .link-shortener button {
        font-size: 1.5rem;
        padding: 0.375rem 1rem 0.35rem 1rem;
        background-color: #FE9DF5;
        font-weight: 600;
        color: #0F0F22;
        /* border: none; */
        cursor: pointer;
        white-space: nowrap;
        border-top-right-radius: 5px;
        border-bottom-right-radius: 5px;
    }
</style>