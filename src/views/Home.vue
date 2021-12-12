<template>
  <div class="home">
      <h1>Project Planner</h1>
      <FilterNav @filterBy="current=$event" :current="current"></FilterNav>
      <div v-for="project in filterProjectByName" :key="project.id">
        <SingleProject :project="project" @complete="completeProject" @delete="deleteProject"></SingleProject>
      </div>
  </div>
  <!-- {{current}} -->
</template>

<script>

import FilterNav from '../components/FilterNav'
import SingleProject from '../components/SingleProject'
export default {
  name: 'Home',
  data(){
    return{
      projects: [],
      current: 'all'
    }
  },
  components: {
    FilterNav,
    SingleProject,
  },
  methods: {
    deleteProject(id){
      this.projects = this.projects.filter((project) => {
        return project.id != id;
      })
    },
    completeProject(id){
      let findProjectId = this.projects.find((project)=>{
        return project.id === id;
      })
      findProjectId.isCompleted =! findProjectId.isCompleted;
    }
  },
  computed:{
    filterProjectByName(){
      if(this.current === 'complete'){
        return this.projects.filter((project)=>{
          return project.isCompleted
        })
      }
      if(this.current === 'ongoing'){
        return this.projects.filter((project)=>{
          return !project.isCompleted
        })
      }
      return this.projects
    }
  },
  mounted(){
      fetch('http://localhost:3000/projects')
      .then((response) => response.json())
      .then(data => this.projects = data)
    }
}
</script>

<style>

</style>
