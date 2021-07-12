<template>
  <div>
    <h1>建立提醒</h1>
      <table class="center">
        <tr>
            <td class="labelright">主題</td>
            <td class="inputleft">{{ this.meeting.topic }}</td>
        </tr>
        <tr>
            <td class="labelright">主持人</td>
            <td class="inputleft">{{ this.meeting.host }}</td>
        </tr>
        <tr>
            <td class="labelright">開始</td>
            <td class="inputleft">{{ this.meeting.start }}</td>
        </tr>
        <tr>
            <td class="labelright">結束</td>
            <td class="inputleft">{{ this.meeting.end }}</td>
        </tr>
        <tr>
            <td class="labelright">會議室</td>
            <td class="inputleft">{{ this.meeting.room }}</td>
        </tr>
        <tr>
            <td class="labelright">參與人</td>
            <td class="inputleft">{{ this.meeting.attendee }}</td>
        </tr>
        <tr>
            <td class="labelright"><label for="remind">提醒</label></td>
            <td class="inputleft"><input id="remind" type="text" v-model="remind">分鐘前</td>
        </tr>
        <tr>
            <td></td>
            <td><button @click='create'>建立</button></td>
        </tr>
      </table>
  </div>
</template>

<script>
export default {
  name: 'CreateReminder',
  props: ['meeting'],
  methods: {
    create() {
      // sent request to API
      const formdata = new FormData();
      formdata.append('id', this.meeting.id);
      formdata.append('reminder_time', this.remind);
      fetch('http://localhost:7000/test/v1/google_reminder_create', {
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
          console.log('create success');
          this.$emit('full-meeting-info');
        } else console.log('create fail');
      }).catch((error) => {
        console.error(error);
      });
    },
  },
};
</script>
