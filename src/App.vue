<script setup lang="ts">
import { ScheduleXCalendar } from '@schedule-x/vue'
import {
  createCalendar,
  viewDay,
  viewMonthAgenda,
  viewMonthGrid,
  viewWeek,
} from '@schedule-x/calendar'
import '@schedule-x/theme-default/dist/index.css'
import { createDragAndDropPlugin } from '@schedule-x/drag-and-drop'
import { createScrollControllerPlugin } from '@schedule-x/scroll-controller'
import {
  createEventRecurrencePlugin,
  createEventsServicePlugin,
} from '@schedule-x/event-recurrence'
import { createEventModalPlugin } from '@schedule-x/event-modal'
import { calendars } from './calendars.ts'
import { ref, shallowRef } from 'vue'
import { createCalendarControlsPlugin } from '@schedule-x/calendar-controls'
import EventBadge from './components/EventBadge.vue'
import ActionsEventModal from './components/ActionsEventModal.vue'

const eventsService = createEventsServicePlugin()
const calendarControls = createCalendarControlsPlugin()
const eventModal = createEventModalPlugin()

const events = ref([
  {
    id: 1,
    title: 'Event 1',
    start: '2025-04-06 12:00',
    end: '2025-04-06 15:00',
    color: 'green',
  },
  {
    id: 2,
    title: 'Event 2',
    start: '2025-04-06',
    end: '2025-04-06',
    color: 'red',
  },
])

const addEvent = ({
  id,
  title,
  color,
  start,
  end,
}: {
  id: number
  title: string
  color: string
  start: string
  end: string
}) => {
  events.value.push({
    id,
    title,
    color,
    start,
    end,
  })
}

const actionsEventModalShow = ref(false)

const calendarApp = shallowRef(
  createCalendar({
    selectedDate: '',
    locale: 'en-UK',
    views: [viewMonthAgenda, viewMonthGrid, viewWeek, viewDay],
    defaultView: viewMonthGrid.name,
    calendars: calendars,
    plugins: [
      eventModal,
      createDragAndDropPlugin(),
      createScrollControllerPlugin({
        initialScroll: '07:00',
      }),
      createEventRecurrencePlugin(),
      eventsService,
      calendarControls,
    ],
    events: events.value,
    monthGridOptions: {
      nEventsPerDay: 3,
    },
    callbacks: {
      onClickDate(date) {
        actionsEventModalShow.value = true
      },
      // onClickDateTime(date) {
      // calendarControls.setView(viewDay.name)
      // calendarControls.setDate(date)
      // },
    },
  })
)

const closeModal = () => {
  eventModal.close()
}

const eventStyles = {
  width: '100%',
  height: '100%',
  backgroundColor: 'white',
  border: '2px solid black',
  borderRadius: '4px',
  padding: '0 4px',
}

const eventModalStyles = {
  boxShadow: '0 0 2em #123',
  backgroundColor: 'white',
  border: '2px solid black',
  borderRadius: '4px',
  padding: '0 4px',
}
</script>

<template>
  <div>
    <ScheduleXCalendar :calendar-app="calendarApp">
      <template #monthAgendaEvent="{ calendarEvent }">
        <EventBadge
          :color="calendarEvent.color"
          :title="calendarEvent.title"
          :startDate="calendarEvent.start"
          :endDate="calendarEvent.end"
        />
      </template>

      <template #dateGridEvent="{ calendarEvent }">
        <EventBadge
          :color="calendarEvent.color"
          :title="calendarEvent.title"
          :startDate="calendarEvent.start"
          :endDate="calendarEvent.end"
        />
      </template>

      <template #timeGridEvent="{ calendarEvent }">
        <EventBadge
          :color="calendarEvent.color"
          :title="calendarEvent.title"
          :startDate="calendarEvent.start"
          :endDate="calendarEvent.end"
        />
      </template>

      <template #monthGridEvent="{ calendarEvent, hasStartDate }">
        <div :style="{ background: 'red' }"></div>
        <EventBadge
          :color="calendarEvent.color"
          :title="calendarEvent.title"
          :startDate="calendarEvent.start"
          :endDate="calendarEvent.end"
        />
      </template>
      <!-- <template #eventModal="{ calendarEvent }">
        <div :style="eventModalStyles">
          {{ calendarEvent.title }}
          {{ calendarEvent }}

          <button @click="closeModal"></button>
        </div>
      </template> -->
    </ScheduleXCalendar>
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
