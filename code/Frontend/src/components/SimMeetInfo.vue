<template>
    <h6
      @click="$emit('full-meeting-info', meeting)"
      class="meeting">
      {{ shortInfo }}
    </h6>
</template>

<script>
export default {
  name: 'SimMeetInfo',
  props: ['meeting'],
  computed: {
    shortInfo() {
      let temp;
      if (this.meeting !== undefined) {
        // should be the meeting without id??
        const myDate = new Date(this.meeting.start);
        const { topic } = this.meeting;
        temp = `${topic}@${myDate.getHours()}:${myDate.getMinutes()}`;
      } else {
        temp = '';
      }
      return temp;
    },
  },
};
</script>

<style scoped>
.meeting{
  height:20%;
  border-bottom: 2px solid rgb(0, 38, 255);
  padding: 3px;
  margin: 3px;
}
</style>
