<template>
    <div>
        <app-tweet v-for="tweet in tweets" :tweet="tweet" :key="tweet.id " />
    </div>
</template>

<script>
import axios from 'axios'
export default {
    data() {
        return{
            tweets: []
        }
    },

    methods:{
        async getTweets(){
            let response = await axios.get('/api/timeline');

            this.tweets = response.data.data
        }
    },

    mounted() {
        this.getTweets()
    },
}
</script>
