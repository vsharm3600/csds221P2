<template>
  <div id="app">
    <ToDo />
  </div>
</template>

<script>
import ToDo from './components/toDo';

export default {
  name: 'App',

  components: {
    ToDo,
  },

  data: () => ({
    //
  }),
};
</script>
