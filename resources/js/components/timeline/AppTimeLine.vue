<template>
    <div>
        <app-tweet v-for="tweet in tweets" :tweet="tweet" :key="tweet.id " />

        <div v-if="tweets.length" v-observe-visibility="handleScrolledToBottomTimeline"></div>
    </div>
</template>

<script>
    import { mapGetters, mapActions } from 'vuex'

    export default {
        data() {
            return {
                page: 1,
                lastPage: 3
            }
        },

        computed: {
            ...mapGetters({
                tweets:('timeline/tweets')
            }),

            urlWithPage() {
                return `/api/timeline/?page=${this.page}`
            }
        },

        methods: {
            ...mapActions({
                getTweets: 'timeline/getTweets'
            }),

            loadTweets() {
                this.getTweets(this.urlWithPage).then((response) => {
                    this.lastPage = response.data.meta.last_page
                })
            },

            handleScrolledToBottomTimeline(isVisible) {
                if(!isVisible) {
                    return
                }
                if(this.lastPage === this.page){
                    return
                }

                this.page++
                this.loadTweets()
            }
        },

        mounted() {
            this.loadTweets()
        },
    }
</script>
