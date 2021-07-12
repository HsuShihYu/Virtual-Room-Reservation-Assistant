<template>
  <div>
    <button @click="$emit('disinvite', attendee)">X</button><label>{{ attendee.user }}</label>
  </div>
</template>

<script>
export default {
  name: 'CancelUser',
  props: ['attendee'],
};
</script>

<style>

</style>
