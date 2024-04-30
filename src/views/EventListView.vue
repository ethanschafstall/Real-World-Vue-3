<script setup>
import EventCard from '@/components/EventCard.vue'
import { onMounted, ref, computed } from 'vue'
import EventService from '@/services/EventService.js'

const props = defineProps(['page'])
const page = computed(() => props.page)

const events = ref(null)

onMounted(() => {
  EventService.getEvents(2, page.value)
    .then((reponse) => {
      events.value = reponse.data
    })
    .catch((error) => {
      console.log(error)
    })
})
</script>

<template>
  <main>
    <h1>Events For Good</h1>
    <h1 v-show="showExtra">Extra Stuff</h1>
    <div class="events">
      <EventCard v-for="event in events" :key="event.id" :event="event" />
      <router-link
        id="page-prev"
        :to="{ name: 'event-list', query: { page: page - 1 } }"
        rel="prev"
        v-if="page != 1"
        >&#60; Previous</router-link
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
