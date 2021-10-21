<template>
  <transition-group name="job-list">
    <div v-for="job in jobs" :key="job.id">
      <SingleJob
        :job="job"
        @add-filter="toggleFilter"
        :categories="categories"
      />
    </div>
  </transition-group>
</template>

<script>
import SingleJob from "./SingleJob.vue";
export default {
  components: { SingleJob },
  emits: ["apply-filter"],
  props: ["jobs", "categories"],
  setup(props, context) {
    const toggleFilter = (category) => {
      context.emit("apply-filter", category);
    };

    return { toggleFilter };
  },
};
</script>

<style>
.job-list-enter-from,
.job-list-leave-to {
  opacity: 0;
  transform: translateX(-30px);
}
.job-list-enter-active {
  transition: all 1s ease-out;
}
.job-list-leave-active {
  transition: all 1s ease-out;
  position: absolute;
}
.job-list-enter-to,
.job-list-leave-from {
  opacity: 1;
  transform: translateX(0);
}
.job-list-move {
  transition: transform 0.8s ease;
}
</style>
