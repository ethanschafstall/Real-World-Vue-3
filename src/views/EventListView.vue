<script setup>
import EventCard from '@/components/EventCard.vue'
import { onMounted, ref, computed, watchEffect, defineProps } from 'vue'
import EventService from '@/services/EventService.js'

const props = defineProps(['page'])
const page = computed(() => props.page)

const events = ref(null)

onMounted(() => {
  watchEffect(() => {
    events.value = null
    EventService.getEvents(4, page.value)
      .then((response) => {
        events.value = response.data
        totalEvents.value = response.headers['x-total-count']
      })
      .catch((error) => {
        console.log(error)
      })
  })
})
</script>

<template>
  <main>
    <h1>Events For Good</h1>
    <h1 v-show="showExtra">Extra Stuff</h1>
    <div class="events">
      <EventCard v-for="event in events" :key="event.id" :event="event" />
      <RouterLink
        id="page-prev"
        :to="{ name: 'event-list', query: { page: page - 1 } }"
        rel="prev"
        v-if="page != 1"
        >&#60; Previous</RouterLink
      >
      <RouterLink :to="{ name: 'event-list', query: { page: page + 1 } }" rel="next"
        >Next Page &#62;</RouterLink
      >
    </div>
  </main>
</template>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
