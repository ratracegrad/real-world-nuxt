<template>
    <div>
        <h1>Events</h1>
        <EventCard
            v-for="(event, $eventIndex) in events"
            :key="$eventIndex"
            :event="event"
            :data-index="$eventIndex"
        ></EventCard>
    </div>
</template>

<script>
import { mapState } from 'vuex';
import EventCard from '@/components/EventCard.vue';

export default {
    head() {
        return {
            title: 'Event Listing'
        };
    },
    components: {
        EventCard
    },
    computed: mapState({
        events: state => state.events.events
    }),
    // asyncData({ $axios, error }) {
    //     return $axios
    //         .get('http://localhost:3000/events')
    //         .then(response => {
    //             return {
    //                 events: response.data
    //             };
    //         })
    //         .catch(e => {
    //             error({
    //                 statusCode: 503,
    //                 message:
    //                     'Unable to fetch events at this time. Please try again.'
    //             });
    //         });
    // }
    async fetch({ store, error }) {
        try {
            await store.dispatch('events/fetchEvents');
        } catch (err) {
            error({
                statusCode: 503,
                message:
                    'Unable to fetch events at this time. Please try again.'
            });
        }
    }
};
</script>

<style>
.container {
    margin: 0 auto;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
}

.title {
    font-family: 'Quicksand', 'Source Sans Pro', -apple-system,
        BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', Arial,
        sans-serif;
    display: block;
    font-weight: 300;
    font-size: 100px;
    color: #35495e;
    letter-spacing: 1px;
}

.subtitle {
    font-weight: 300;
    font-size: 42px;
    color: #526488;
    word-spacing: 5px;
    padding-bottom: 15px;
}

.links {
    padding-top: 15px;
}
</style>
