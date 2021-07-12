<template>
  <div>
    <h1>{{ createText }}會議</h1>
    <table class="center">
      <tr>
          <td class="labelright"><label for="topic">主題</label></td>
          <td class="inputleft">
            <input id="topic" type="text" v-model="topic">
          </td>
      </tr>
      <tr>
          <td class="labelright"><label for="host">主持人</label></td>
          <td class="inputleft">
            <input id="host" type="text" v-model="host">
          </td>
      </tr>
      <tr>
          <td class="labelright"><label for="start">開始</label></td>
          <td class="inputleft">
            <input id="start" type="datetime-local" v-model="start">
          </td>
      </tr>
      <tr>
          <td class="labelright"><label for="end">結束</label></td>
          <td class="inputleft">
            <input id="end" type="datetime-local" v-model="end">
          </td>
      </tr>
      <tr>
          <td class="labelright"><label for="room">會議室</label></td>
          <td class="inputleft">
            <input id="room" type="text" v-model="room">
          </td>
      </tr>
      <tr>
          <td class="labelright"><label for="attendee">參與人</label></td>
          <td class="inputleft">
            <input id="attendee" type="text" v-model="attendee">
          </td>
      </tr>
      <tr>
          <td>{{ update }}</td>
          <td><button type="button" @click="create">{{ createText }}</button></td>
      </tr>
    </table>
  </div>
</template>

<script>
// import CancelUser from './CancelUser.vue';

export default {
  name: 'CreateMeeting',
  props: ['meeting'],
  // components: {
  //   CancelUser,
  // },
  data() {
    return {
      currentMeeting: this.meeting,
      update: this.updateValue(),
    };
  },
  methods: {
    create() {
      // call the api to store the meeting
      // collect data from the from
      // distinguish between edit and create
      if (this.meeting.id === undefined) {
        // for create
        const formdata = new FormData();
        formdata.append('topic', this.topic);
        formdata.append('host', this.host);
        // time format need debug
        formdata.append('start', this.start);
        formdata.append('end', this.end);
        formdata.append('room', this.room);
        formdata.append('attendee', this.attendee);
        fetch('http://localhost:7000/test/v1/meeting_create', {
          method: 'POST',
          mode: 'cors',
          credentials: 'same-origin',
          body: formdata,
        }).then((response) => {
          if (!response.ok) {
            throw new Error('fetch error!!');
          } else {
            return response.json();
          }
        }).then((jsonResponse) => {
          if (jsonResponse.result === 0) {
            // operation successful, pass the current data and id to the next component
            console.log(jsonResponse.result);
            const newMeeting = {
              id: jsonResponse.id,
              topic: this.topic,
              host: this.host,
              start: this.start,
              end: this.end,
              room: this.room,
              attendee: this.attendee,
            };
            this.$emit('create-reminder', newMeeting);
          } else alert('create fail');
        }).catch((error) => {
          console.error(error);
        });
      } else {
        // for editing
        const formdata = new FormData();
        formdata.append('id', this.meeting.id);
        formdata.append('topic', this.topic);
        formdata.append('host', this.host);
        // time format need debug
        formdata.append('start', this.start);
        formdata.append('end', this.end);
        formdata.append('room', this.room);
        formdata.append('attendee', this.attendee);
        fetch('http://localhost:7000/test/v1/meeting_edit', {
          method: 'POST',
          mode: 'cors',
          credentials: 'same-origin',
          body: formdata,
        }).then((response) => {
          if (!response.ok) {
            throw new Error('fetch error!!');
          } else {
            return response.json();
          }
        }).then((jsonResponse) => {
          if (jsonResponse.result === 0) {
            // operation successful, send another meeting request
            alert('edit success');
          } else {
            alert('edit fail');
          }
        }).catch((error) => {
          console.error(error);
        });
      }
    },
    updateValue() {
      this.topic = this.meeting.topic;
      this.host = this.meeting.host;
      this.start = this.meeting.start;
      this.end = this.meeting.end;
      this.room = this.meeting.room;
      this.attendee = this.meeting.attendee;
    },
  },
  computed: {
    createText() {
      if (this.meeting.id !== undefined) {
        this.updateValue();
        return '更新';
      }
      return '建立';
    },
  },
};
</script>
