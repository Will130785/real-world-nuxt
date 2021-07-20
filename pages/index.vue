<template>
  <div>
    <h1>Events</h1>
    <div v-for="(event, index) in events" :key="index">
      <EventCard :event="event" />
    </div>
  </div>
</template>

<script>
import EventCard from '@/components/EventCard.vue'
import { mapState } from 'vuex'
export default {
  components: {
    EventCard
  },
  head () {
    return {
      title: 'Event Listing'
    }
  },
  // asyncData ({ $axios, error }) {
  //   return $axios.get('http://localhost:3000/events').then(response => {
  //     console.log(response.data)
  //     return {
  //       events: response.data
  //     }
  //   })
  //   .catch(e => {
  //     error({ statusCode: 503, message: 'Unable to fetch events at this time. Please try again.' })
  //   })
  // }
  async fetch ({ store, error }) {
    try {
      await store.dispatch('events/fetchEvents')
    } catch (err) {
      error({
        statusCode: 503,
        message: 'Unable to fetch events at this time. Please try again'
      })
    }
  },
  computed: mapState({
    events: state => state.events.events
  })
}
</script>