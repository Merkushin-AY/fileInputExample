<template>
  <main id="app" class="main">
    <div class="container">
        <FileFormExample/>
    </div>
  </main>
</template>

<script>

import FileFormExample from '@/components/forms/FileFormExample';

export default {
  name: 'App',
  components: {
      FileFormExample,
  }
}
</script>

<style>
@import './assets/styles/main.css';

.main {
    width: 100%;
    min-height: 100vh;
    padding: 32px 0;
    background: rgb(145,163,255);
    background: linear-gradient(90deg, rgba(145,163,255,1) 0%, rgba(255,175,192,1) 100%);
}
</style>
