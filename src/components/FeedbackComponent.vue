<template>
  <div>
    <h2>Feedback</h2>
    <p>{{ feedback }}</p>
  </div>
</template>

<script>
export default {
  props: ['feedback'],
};
</script>

<style scoped>
/* Scoped CSS for FeedbackComponent */
h2 {
  font-size: 24px;
  color: #007BFF;
}
p {
  font-size: 18px;
  line-height: 1.6;
}
</style>
