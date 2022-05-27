<template>
  <div class="section">
    <h2 class="title">{{ title }}</h2>
    <h3 class="title">Same App with different frameworks or languages</h3>
    <div class="container">
      <div v-for="project in projects" class="card" :key="project.id">
        <ProjectsCard :project="project" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import ProjectsCard from '../components/ProjectsCard.vue';

export default {
  components: {
    ProjectsCard,
  },
  mounted() {
    this.getData();
  },
  data() {
    return {
      url: 'https://assitant-app.netlify.app/api/projects-api',
      projects: [],
    };
  },
  computed: {
    title() {
      return this.projects.length > 0 ? 'Assistant' : 'Loading...';
    },
  },
  methods: {
    async getData() {
      try {
        const response = await axios.get(this.url);
        const data = await response.data;
        this.projects = data;
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style></style>
