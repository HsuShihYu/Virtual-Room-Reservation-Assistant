<template>
    <!--make it a component-->
    <div>
        <h1>會議室資訊</h1>
        <h3>{{ currentMonth }}</h3>
        <table class="calendar">
          <!-- date number column-->
          <tr>
            <td></td>
            <td class="dateRow">Sun</td>
            <td class="dateRow">Mon</td>
            <td class="dateRow">Tue</td>
            <td class="dateRow">Wed</td>
            <td class="dateRow">Thr</td>
            <td class="dateRow">Fri</td>
            <td class="dateRow">Sat</td>
          </tr>
          <tr>
            <td></td>
            <DateInfo
              v-for="date in calWeek"
              :key="date.id"
              :date="date.number"
            >
            </DateInfo>
          </tr>
          <tr v-for="room in meetings" :key="room.location">
            <td>{{ room.location }}</td>
            <DateInfo
              :meetings="room.meetings[0]"
              @full-meeting-info="$emit('full-meeting-info', $event)">
            </DateInfo>
            <DateInfo
              :meetings="room.meetings[1]"
              @full-meeting-info="$emit('full-meeting-info', $event)">
            </DateInfo>
            <DateInfo
              :meetings="room.meetings[2]"
              @full-meeting-info="$emit('full-meeting-info', $event)">
            </DateInfo>
            <DateInfo
              :meetings="room.meetings[3]"
              @full-meeting-info="$emit('full-meeting-info', $event)">
            </DateInfo>
            <DateInfo
              :meetings="room.meetings[4]"
              @full-meeting-info="$emit('full-meeting-info', $event)">
            </DateInfo>
            <DateInfo
              :meetings="room.meetings[5]"
              @full-meeting-info="$emit('full-meeting-info', $event)">
            </DateInfo>
            <DateInfo
              :meetings="room.meetings[6]"
              @full-meeting-info="$emit('full-meeting-info', $event)">
            </DateInfo>
          </tr>
        </table>
        <button
        @click="decWeek"
        class="bottombutton"
        >
          <img
            src="../assets/last.png"
            class="buttonimage"
          >
        </button>
        {{ currentYear }}/{{ currentMonth }}/{{ currentDay }}
        <button
          @click="incWeek"
          class="bottombutton"
        >
          <img
            src="../assets/next.png"
            class="buttonimage"
          >
        </button>
    </div>
</template>

<script>
import DateInfo from './DateInfo.vue';

export default {
  name: 'MeetingRoomInfo.vue',
  props: ['initialdate'],
  data() {
    return {
      currentDate: this.initialdate,
      // since Vue.js don't detect the change of date Object, we have to do this manually
      currentDay: this.initialdate.getDate(),
      currentMonth: this.initialdate.getMonth() + 1,
      currentYear: this.initialdate.getFullYear(),
      calWeek: this.weekGenerator(this.initialdate),
      meetings: [
        { location: 'Room1', meetings: [] },
        { location: 'Room2', meetings: [] },
        { location: 'Room3', meetings: [] },
        { location: 'Room4', meetings: [] },
        { location: 'Room5', meetings: [] },
      ],
      // on developement state, we have only 5 room, so Array(5)
    };
  },
  methods: {
    async meetingRequest(date) {
      // get meeting list of the specific month from server
      const formdata = new FormData();
      // set the date to the beginning of the week
      const year = date.getFullYear();
      const month = date.getMonth() + 1;
      const day = date.getDate() - date.getDay();
      formdata.append('year', year);
      formdata.append('month', month);
      formdata.append('day', day);
      return fetch('http://localhost:7000/test/v1/get_meeting_info_week', {
        method: 'POST',
        body: formdata,
        credentials: 'same-origin',
        mode: 'cors',
      }).then(async (response) => {
        if (!response.ok) {
          throw new Error('fetch error!!');
        } else {
          const jsonResponse = await response.json();
          return jsonResponse;
        }
      }).then(async (jsonResponse) => {
        const myMeetings = [];
        let i;
        let j;
        for (i = 0; i <= 4; i += 1) {
          // creating 5 room, and seven days
          myMeetings.push({ location: `Room${i + 1}`, meetings: [] });
          for (j = 0; j <= 6; j += 1) {
            myMeetings[i].meetings.push(jsonResponse[i + 1][j + 1].meeting);
          }
        }
        this.meetings = myMeetings;
        this.$forceUpdate();
      }).catch((error) => {
        console.error(error);
      });
    },
    incWeek() {
      this.currentDate.setDate(this.currentDate.getDate() + 7);
      this.currentMonth = this.currentDate.getMonth() + 1;
      this.currentDay = this.currentDate.getDate();
      this.calWeek = this.weekGenerator(this.currentDate);
    },
    decWeek() {
      this.currentDate.setDate(this.currentDate.getDate() - 7);
      this.currentMonth = this.currentDate.getMonth() + 1;
      this.currentDay = this.currentDate.getDate();
      this.calWeek = this.weekGenerator(this.currentDate);
    },
    weekGenerator(date) {
      // return the array of all of the date in the week for the currentDate
      // in the form of [{id:0, date:1}, ..., {id:6, date:7}]
      // buffer the current date
      const initYear = date.getFullYear();
      const initMonth = date.getMonth();
      const initDate = date.getDate();
      // set the current date to the first day of the week
      date.setDate(date.getDate() - date.getDay());
      const calWeek = [];
      let i = 0;
      for (;i <= 6; i += 1) {
        calWeek.push({ id: i, number: date.getDate() });
        date.setDate(date.getDate() + 1);
      }
      // reset the date
      date.setYear(initYear);
      date.setMonth(initMonth);
      date.setDate(initDate);
      this.meetings = this.meetingRequest(date);
      return calWeek;
    },
  },
  components: {
    DateInfo,
  },
};
</script>

<style>

</style>
