<template>
    <div>
      <h1>日曆</h1>
        <h3>{{ currentMonth }}</h3>
        <table class='calendar'>
            <tr>
                <td class="dateRow">Sun</td>
                <td class="dateRow">Mon</td>
                <td class="dateRow">Tue</td>
                <td class="dateRow">Wed</td>
                <td class="dateRow">Thr</td>
                <td class="dateRow">Fri</td>
                <td class="dateRow">Sat</td>
            </tr>
            <tr>
            <DateInfo
              v-for="date in cal[0]"
              :key="date.id"
              :date='date.number'
            >
            </DateInfo>
            </tr>
            <tr>
            <DateInfo
              v-for="date in cal[0]"
              :key="date.id"
              :meetings="meetings[date.number - 1]"
              @full-meeting-info="$emit('full-meeting-info', $event)">
            </DateInfo></tr>

            <tr>
            <DateInfo
              v-for="date in cal[1]"
              :key="date.id"
              :date='date.number'
            >
            </DateInfo>
            </tr>
            <tr>
            <DateInfo
              v-for="date in cal[1]"
              :key="date.id"
              :meetings="meetings[date.number - 1]"
              @full-meeting-info="$emit('full-meeting-info', $event)">
            </DateInfo></tr>

            <tr>
            <DateInfo
              v-for="date in cal[2]"
              :key="date.id"
              :date='date.number'
            >
            </DateInfo>
            </tr>
            <tr>
            <DateInfo
              v-for="date in cal[2]"
              :key="date.id"
              :meetings="meetings[date.number - 1]"
              @full-meeting-info="$emit('full-meeting-info', $event)">
            </DateInfo></tr>

            <tr>
            <DateInfo
              v-for="date in cal[3]"
              :key="date.id"
              :date='date.number'
            >
            </DateInfo>
            </tr>
            <tr>
            <DateInfo
              v-for="date in cal[3]"
              :key="date.id"
              :meetings="meetings[date.number - 1]"
              @full-meeting-info="$emit('full-meeting-info', $event)">
            </DateInfo></tr>

            <tr>
            <DateInfo
              v-for="date in cal[4]"
              :key="date.id"
              :date='date.number'
            >
            </DateInfo>
            </tr>
            <tr>
            <DateInfo
              v-for="date in cal[4]"
              :key="date.id"
              :meetings="meetings[date.number - 1]"
              @full-meeting-info="$emit('full-meeting-info', $event)">
            </DateInfo></tr>
        </table>
          <button
            @click="decMonth"
            class="bottombutton">
            <img
              src="../assets/last.png"
              class="buttonimage">
          </button>
          {{ currentYear }}/{{ currentMonth }}/{{currentDay}}
          <button
            @click="incMonth"
            class="bottombutton">
            <img
              src="../assets/next.png"
              class="buttonimage">
          </button>
    </div>
</template>

<script>
import DateInfo from './DateInfo.vue';

export default {
  name: 'Calendar',
  props: ['initialdate'],
  components: {
    DateInfo,
  },
  data() {
    return {
      currentDate: this.initialdate,
      currentDay: this.initialdate.getDate(),
      currentMonth: this.initialdate.getMonth() + 1,
      currentYear: this.initialdate.getFullYear(),
      cal: this.calGenerator(this.initialdate),
      meetings: new Array(31),
    };
  },
  methods: {
    // since different calendar needs different form of meeting,
    // so the meetingRequest put here
    async meetingRequest(date) {
      // production usage
      const formdata = new FormData();
      const year = date.getFullYear();
      const month = date.getMonth() + 1;
      formdata.append('year', year);
      formdata.append('month', month);
      return fetch('http://localhost:7000/test/v1/get_meeting_info_month', {
        method: 'POST',
        body: formdata,
        credentials: 'same-origin',
        mode: 'cors',
      }).then(async (response) => {
        if (!response.ok) {
          throw new Error('fetcht error!!');
        } else {
          const jsonResponse = await response.json();
          return jsonResponse;
        }
      }).then(async (jsonResponse) => {
        let i = 0;
        const meetingForCal = new Array(31);
        for (i = 0; i <= 30; i += 1) {
          meetingForCal[i] = jsonResponse[i + 1];
        }
        this.meetings = meetingForCal;
        this.$forceUpdate();
      }).catch((error) => {
        console.error(error);
      });
    },
    incMonth() {
      this.currentDate.setMonth(this.currentDate.getMonth() + 1);
      this.currentMonth = this.currentDate.getMonth() + 1;
      this.currentYear = this.currentDate.getFullYear();
      this.cal = this.calGenerator(this.currentDate);
    },
    decMonth() {
      this.currentDate.setMonth(this.currentDate.getMonth() - 1);
      this.currentMonth = this.currentDate.getMonth() + 1;
      this.currentYear = this.currentDate.getFullYear();
      this.cal = this.calGenerator(this.currentDate);
    },
    calGenerator(date) {
      // render the calendar according to the month
      // in the form of
      // [
      //     [sun,mon,tue,wed,thr,fri,sat],
      //     [{id:0, number:2},{id:1, number:3}],
      //     [],
      //     [],
      //     [],
      //     []
      // ]
      // read the day of the first day
      // temporary parameter for passing the object reference mechanism of js...
      const initYear = date.getFullYear();
      const initMonth = date.getMonth();
      const initDate = date.getDate();
      const cal = [];
      // set the date to the first sunday
      date.setDate(1);
      date.setDate(date.getDate() - date.getDay());
      // fill the 7x5 array of cal
      for (let i = 0; i <= 4; i += 1) {
        cal.push([]);
        for (let j = 0; j <= 6; j += 1) {
          cal[i].push({ id: j, number: date.getDate() });
          date.setDate(date.getDate() + 1);
        }
      }
      // recovery the original state
      date.setYear(initYear);
      date.setMonth(initMonth);
      date.setDate(initDate);
      // update the meeting list
      this.meetings = this.meetingRequest(date);
      return cal;
    },
  },
};
</script>
