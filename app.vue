<template>
  <h1>{{ name }}</h1>

  <div v-if="pending">Loading Projects...</div>
  <div>
    <h2>Projects</h2>
    <ul>
      <li v-for="project in projects" :key="project.name">
        <h3>{{ project.name }}</h3>
        <p>{{ project.description }}</p>
      </li>
    </ul>
  </div>

  <div v-if="pending1">Loading Blogs...</div>
  <div>
    <h2>Blogs</h2>
    <ul>
      <li v-for="blog in blogs">
        <h3>{{ blog.title }}</h3>
        <p>{{ blog.content }}</p>
      </li>
    </ul>
  </div>

  <div v-if="pending2">Loading Students...</div>
  <div>
    <h2>Students Info</h2>
    <ul>
      <li v-for="student in students">
        <h3>{{ student.name }}</h3>
        <p>{{ student.studentId }}</p>
        <p>{{ student.age }}</p>
      </li>
    </ul>
  </div>
</template>

<script setup>
const name = "Mithila Rasanjith";
const {
  data: projects,
  pending,
  error,
} = useFetch("http://localhost:5000/projects");

const {
  data: blogs,
  pending1,
  error1,
} = useFetch("http://localhost:5000/blogs");

const {
  data: students,
  pending2,
  error2,
} = useFetch("http://localhost:5000/students");
</script>

<style>

</style>