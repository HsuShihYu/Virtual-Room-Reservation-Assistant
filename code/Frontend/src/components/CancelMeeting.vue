<template>
  <div>
      <h1>取消會議</h1>
      <table class="center">
        <tr>
          <td class="labelright">主題</td>
          <td class="inputleft">{{ meeting.topic }}</td>
        </tr>
        <tr>
          <td class="labelright">開始</td>
          <td class="inputleft">{{ meeting.start }}</td>
        </tr>
        <tr>
          <td class="labelright">結束</td>
          <td class="inputleft">{{ meeting.end }}</td>
        </tr>
        <tr>
          <td></td>
          <td class="inputleft"><button @click="cancelMeeting">發送</button></td>
        </tr>
      </table>
  </div>
</template>

<script>
export default {
  name: 'CancelMeeting',
  props: ['meeting'],
  methods: {
    cancelMeeting() {
      const formdata = new FormData();
      formdata.append('id', this.meeting.id);
      fetch('http://localhost:7000/test/v1/meeting_cancel', {
        method: 'POST',
        mode: 'cors',
        credentials: 'same-origin',
        body: formdata,
      }).then((response) => {
        if (!response.ok) {
          throw new Error('fetch error');
        }
        return response.json();
      }).then((jsonResponse) => {
        if (jsonResponse.result === 0) {
          alert('cancel success');
        } else {
          alert('cancel fail');
        }
      }).catch((error) => {
        console.error(error);
      });
    },
  },
};
</script>

<style>
</style>
