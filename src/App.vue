<template>
  <Header />
  <Home />
</template>

<script>
import Header from '@/components/Header';
import Home from '@/views/Home';

export default {
  name: 'App',
  components: {
    Header,
    Home,
  },
  async created() {
    this.loading = false;
  },
};
</script>
