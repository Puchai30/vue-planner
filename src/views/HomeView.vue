<template>
  <div class="home">
    <h1>Home</h1>
    <FilterNav @filterValue="current=$event" :current_nav="current"></FilterNav>
    <div v-for="project in filterdProjects" :key="project.id">
      <SingleProject
        :single_project="project"
        @delete="deleteProj"
        @complete="completeProj"
      ></SingleProject>
    </div>
  </div>
</template>

<script>
import FilterNav from '../components/FilterNav'
import SingleProject from "../components/SingleProject";

export default {
  name: "HomeView",
  components: {
    FilterNav,
    SingleProject,
  },
  data() {
    return {
      projects: [],
      current: "all"
    };
  },
  methods: {
    deleteProj(id) {
      this.projects = this.projects.filter(project => {
        return project.id !== id;
      });
    },

    completeProj(id) {
      let findProject = this.projects.find(project => {
        return project.id == id;
      });
      findProject.complete =! findProject.complete;
    },
  },

  computed: {
    filterdProjects() {
      if(this.current==="complete"){
        return this.projects.filter((project)=>{
          return project.complete
        })
      }
      if(this.current==="ongoing"){
        return this.projects.filter((project)=>{
          return !project.complete
        })
      }
      return this.projects
    }
  },

  mounted() {
    fetch("http://localhost:3000/projects")
      .then((response) => {
        return response.json();
      })
      .then((datas) => {
        this.projects = datas;
      })
      .catch((error) => {
        console.log("error");
      });
  },
};
</script>
