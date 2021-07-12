<template>
    <td>
      <div v-if="date !== undefined" class="date">{{ date }}</div>
      <div class="meetings" v-else-if="date === undefined">
        <sim-meet-info
          v-for='meeting in meetings'
          :key='meeting.topic'
          :meeting='meeting'
          @full-meeting-info="$emit('full-meeting-info', $event)">
        </sim-meet-info>
      </div>
    </td>
</template>

<script>
import SimMeetInfo from './SimMeetInfo.vue';

export default {
  name: 'DateInfo',
  props: ['date', 'meetings'],
  components: {
    SimMeetInfo,
  },
};
</script>

<style scoped>
  .date{
    padding: 1px;
    margin: 0px;
  }
  .meetings{
    padding: 0px;
    overflow: auto;
  }
  td{
    width: 16%;
  }
</style>
