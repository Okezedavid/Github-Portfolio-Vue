<script setup>
  import gitHubIcon from './icons/gitHubIcon.vue';
</script>

<template>

  <nav>
    <div class="navbar">
      <gitHubIcon class="github-icon"/>
      <h1>DavidOkeze's Github</h1>
    </div>
  </nav>

</template>

<style scoped>

.navbar {
  display: flex;
  align-items: center;
  gap: 10px;
  justify-content: center;
  background-color: #393f4d;
  text-align: center;
  padding: 20px;
  margin-bottom: 100px;
}

.github-icon {
  font-size: 3rem;
}
</style>
