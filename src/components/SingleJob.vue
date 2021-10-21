<template>
  <div class="card job-card" :class="{ featured: job.featured }">
    <div class="details">
      <img :src="require(`@/assets/images/${imgName}`)" alt="" />
      <div class="details-content">
        <div class="details-top">
          <h4>{{ job.company }}</h4>
          <span class="btn btn-new" v-if="job.new">NEW!</span>
          <span class="btn btn-featured" v-if="job.featured">FEATURED</span>
        </div>
        <h2>{{ job.position }}</h2>
        <ul>
          <li>{{ job.postedAt }}</li>
          <li>{{ job.contract }}</li>
          <li>{{ job.location }}</li>
        </ul>
      </div>
    </div>
    <div class="categories">
      <span
        @click="$emit('add-filter', category)"
        v-for="category in categories"
        :key="category"
        class="pill"
        >{{ category }}</span
      >
    </div>
  </div>
</template>

<script>
import { computed } from "@vue/reactivity";

export default {
  props: ["job"],
  emits: ["add-filter"],
  setup(props) {
    const categories = computed(() => [
      props.job.role,
      props.job.level,
      ...props.job.languages,
      ...props.job.tools,
    ]);

    const imgName = computed(() => {
      return props.job.logo.substring(9, props.job.logo.length);
    });

    // for (const key in props.job) {
    //   if (key === "role") {
    //     categories.value.push(props.job[key]);
    //   }
    //   if (key === "level") {
    //     categories.value.push(props.job[key]);
    //   }
    //   if (key === "languages") {
    //     props.job[key].forEach((language) => {
    //       categories.value.push(language);
    //     });
    //   }
    //   if (key === "tools") {
    //     props.job[key].forEach((tool) => {
    //       categories.value.push(tool);
    //     });
    //   }
    // }

    return { imgName, categories };
  },
};
</script>

<style>
.job-card {
  display: flex;
  flex-direction: column;
}

h2 {
  font-size: 2rem;
  margin: 10px 0;
}

@media only screen and (min-width: 700px) {
  .job-card {
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
  }
}

.job-card.featured {
  border-left-color: var(--Desaturated-Dark-Cyan);
}

.details {
  display: flex;
  align-items: center;
  position: relative;
}

.details img {
  display: inline-block;
  margin-right: 2rem;
  position: absolute;
  top: -60px;
  width: 60px;
  height: 60px;
}

@media only screen and (min-width: 700px) {
  .details img {
    position: relative;
    top: 0;
    left: 0;
    width: 80px;
    height: 80px;
  }
}

.details-top {
  display: flex;
  align-items: center;
  margin: 10px 0;
}

.details-top h4 {
  color: var(--Desaturated-Dark-Cyan);
  margin-right: 1rem;
}

.btn {
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 12px;
  text-transform: uppercase;
  color: white;
  padding: 5px 10px;
  font-size: 0.8rem;
  font-weight: 700;
}

.btn-new {
  background-color: var(--Desaturated-Dark-Cyan);
}

.btn-featured {
  background-color: black;
  margin-left: 0.5rem;
}

.categories {
  display: flex;
  flex-wrap: wrap;
}

ul {
  list-style: none;
  display: flex;
  align-items: center;
}

ul li {
  color: var(--Dark-Grayish-Cyan);
  font-weight: 500;
  font-size: 1.2rem;
  margin-left: 5px;
}

ul li:not(:last-child)::after {
  content: "";
  background-color: var(--Dark-Grayish-Cyan);
  width: 4px;
  height: 4px;
  border-radius: 50%;
  display: inline-block;
  margin-left: 5px;
  margin-bottom: 2px;
}

.pill {
  color: var(--Desaturated-Dark-Cyan);
  background: var(--Light-Grayish-Cyan-bg);
  font-weight: 700;
  padding: 1rem;
  font-size: 1.2rem;
  border-radius: 6px;
  margin: 1rem;
  cursor: pointer;
  transition: all 0.3s ease;
}
.pill:hover {
  background: var(--Desaturated-Dark-Cyan);
  color: white;
}
.pill:last-child {
  margin-right: 0;
}
</style>
