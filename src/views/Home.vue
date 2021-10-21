<template>
  <div class="home">
    <div v-if="filteredJobs.length">
      <div
        class="card filter-card"
        :class="{ active: categoriesToFilter.length > 0 }"
      >
        <div class="filter-items">
          <div
            v-for="category in categoriesToFilter"
            :key="category"
            class="filter-item"
          >
            <span class="pill">{{ category }}</span>
            <span @click="removeFilter(category)" class="delete">X</span>
          </div>
        </div>

        <a @click="clearFilters" class="clear">Clear</a>
      </div>
      <ListView :jobs="filteredJobs" @apply-filter="addFilter" />
    </div>
  </div>
</template>

<script>
import ListView from "../components/ListView.vue";
import { computed, ref } from "@vue/reactivity";

export default {
  name: "Home",
  components: { ListView },
  setup() {
    const jobs = ref([
      {
        id: 1,
        company: "Photosnap",
        logo: "./images/photosnap.svg",
        new: true,
        featured: true,
        position: "Senior Frontend Developer",
        role: "Frontend",
        level: "Senior",
        postedAt: "1d ago",
        contract: "Full Time",
        location: "USA Only",
        languages: ["HTML", "CSS", "JavaScript"],
        tools: [],
      },
      {
        id: 2,
        company: "Manage",
        logo: "./images/manage.svg",
        new: true,
        featured: true,
        position: "Fullstack Developer",
        role: "Fullstack",
        level: "Midweight",
        postedAt: "1d ago",
        contract: "Part Time",
        location: "Remote",
        languages: ["Python"],
        tools: ["React"],
      },
      {
        id: 3,
        company: "Account",
        logo: "./images/account.svg",
        new: true,
        featured: false,
        position: "Junior Frontend Developer",
        role: "Frontend",
        level: "Junior",
        postedAt: "2d ago",
        contract: "Part Time",
        location: "USA Only",
        languages: ["JavaScript"],
        tools: ["React", "Sass"],
      },
      {
        id: 4,
        company: "MyHome",
        logo: "./images/myhome.svg",
        new: false,
        featured: false,
        position: "Junior Frontend Developer",
        role: "Frontend",
        level: "Junior",
        postedAt: "5d ago",
        contract: "Contract",
        location: "USA Only",
        languages: ["CSS", "JavaScript"],
        tools: [],
      },
      {
        id: 5,
        company: "Loop Studios",
        logo: "./images/loop-studios.svg",
        new: false,
        featured: false,
        position: "Software Engineer",
        role: "Fullstack",
        level: "Midweight",
        postedAt: "1w ago",
        contract: "Full Time",
        location: "Worldwide",
        languages: ["JavaScript"],
        tools: ["Ruby", "Sass"],
      },
      {
        id: 6,
        company: "FaceIt",
        logo: "./images/faceit.svg",
        new: false,
        featured: false,
        position: "Junior Backend Developer",
        role: "Backend",
        level: "Junior",
        postedAt: "2w ago",
        contract: "Full Time",
        location: "UK Only",
        languages: ["Ruby"],
        tools: ["RoR"],
      },
      {
        id: 7,
        company: "Shortly",
        logo: "./images/shortly.svg",
        new: false,
        featured: false,
        position: "Junior Developer",
        role: "Frontend",
        level: "Junior",
        postedAt: "2w ago",
        contract: "Full Time",
        location: "Worldwide",
        languages: ["HTML", "JavaScript"],
        tools: ["Sass"],
      },
      {
        id: 8,
        company: "Insure",
        logo: "./images/insure.svg",
        new: false,
        featured: false,
        position: "Junior Frontend Developer",
        role: "Frontend",
        level: "Junior",
        postedAt: "2w ago",
        contract: "Full Time",
        location: "USA Only",
        languages: ["JavaScript"],
        tools: ["Vue", "Sass"],
      },
      {
        id: 9,
        company: "Eyecam Co.",
        logo: "./images/eyecam-co.svg",
        new: false,
        featured: false,
        position: "Full Stack Engineer",
        role: "Fullstack",
        level: "Midweight",
        postedAt: "3w ago",
        contract: "Full Time",
        location: "Worldwide",
        languages: ["JavaScript", "Python"],
        tools: ["Django"],
      },
      {
        id: 10,
        company: "The Air Filter Company",
        logo: "./images/the-air-filter-company.svg",
        new: false,
        featured: false,
        position: "Front-end Dev",
        role: "Frontend",
        level: "Junior",
        postedAt: "1mo ago",
        contract: "Part Time",
        location: "Worldwide",
        languages: ["JavaScript"],
        tools: ["React", "Sass"],
      },
    ]);
    const categoriesToFilter = ref([]);

    const addFilter = (category) => {
      if (categoriesToFilter.value.indexOf(category) === -1) {
        categoriesToFilter.value.push(category);
      }
    };

    const removeFilter = (category) => {
      const categoryIndex = categoriesToFilter.value.indexOf(category);
      categoriesToFilter.value.splice(categoryIndex, 1);
    };

    const clearFilters = () => {
      if (categoriesToFilter.value.length === 0) {
        return;
      }
      categoriesToFilter.value = [];
    };

    const filteredJobs = computed(() => {
      if (categoriesToFilter.value.length === 0) {
        return jobs.value;
      }
      return jobs.value.filter((job) => {
        let hasCategory = true;
        const jobCategories = [
          ...job.languages,
          job.role,
          job.level,
          ...job.tools,
        ];
        categoriesToFilter.value.forEach((category) => {
          hasCategory = hasCategory && jobCategories.indexOf(category) >= 0;
        });
        return hasCategory;
      });
    });

    return {
      categoriesToFilter,
      addFilter,
      removeFilter,
      clearFilters,
      filteredJobs,
    };
  },
};
</script>

<style scoped>
.home {
  margin: 120px auto;
  position: relative;
  z-index: 99;
}

.filter-items {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  gap: 30px;
}

.filter-item {
  display: flex;
  align-items: stretch;
}

.filter-card {
  min-height: 60px;
  visibility: hidden;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.filter-card.active {
  visibility: visible;
}

.delete {
  border-top-right-radius: 6px;
  border-bottom-right-radius: 6px;
  background: var(--Desaturated-Dark-Cyan);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 15px;
  cursor: pointer;
  color: white;
}

.delete:hover {
  background: black;
}

.pill {
  margin: 0;
  display: flex;
  align-items: center;
  border-top-right-radius: 0;
  border-bottom-right-radius: 0;
}
.pill:hover {
  background: none;
  color: initial;
}
.clear {
  font-weight: 700;
  color: var(--Dark-Grayish-Cyan);
}
.clear:hover {
  text-decoration: underline;
  color: var(--Desaturated-Dark-Cyan);
  cursor: pointer;
}
</style>
