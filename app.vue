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

  <div>
    <h2>Create a new project</h2>
    <form @submit.prevent="createProject">
      <div>
      <label for="name">Name</label>
      <input type="text" v-model="newProject.name" placeholder="Name" />
      </div><br>
      <div>
      <label for="description">Description</label>
      <input type="text" v-model="newProject.description" placeholder="Description" />
      </div><br>
      <button type="submit">Create Project</button>
      <br>
    </form>
  </div>

  <div>
    <h2>Delete a project</h2>
    <form @submit.prevent="deleteProject">
      <div>
      <label for="id">Id</label>
      <input type="text" v-model="deleteProjectId" placeholder="Id" />
      </div><br>
      <button type="submit">Delete Project</button>
      <br>
    </form>
  </div>
  
  <div>
    <h2>Update a project</h2>
    <form @submit.prevent="updateProject">
      <div>
      <label for="id">Id</label>
      <input type="text" v-model="updateProjectId" placeholder="Id" />
      </div><br>
      <div>
      <label for="name">Name</label>
      <input type="text" v-model="tempProject.name" placeholder="Name" />
      </div><br>
      <div>
      <label for="description">Description</label>
      <input type="text" v-model="tempProject.description" placeholder="Description" />
      </div><br>
      <button type="submit">Update Project</button>
      <br>
    </form>
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

const newProject = ref({
  name: '',
  description: '',
});

const deleteProjectId = ref('');
const updateProjectId = ref('');
const tempProject = ref({
  name: '',
  description: '',
});

const createProject = async () => {
  console.log(newProject.value);

  try {
    const response = await fetch('http://localhost:5000/projects', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(newProject.value),
    });

    if (!response.ok) {
      throw new Error('Failed to create project');
    }

    const result = await response.json();
    projects.value.push(result); // Add the new project to the list of projects

    // Clear the form
    newProject.value.name = '';
    newProject.value.description = '';
    
  } catch (error) {
    console.log('Error', error);
  }
}

const deleteProject = async () => {

  const id = deleteProjectId.value;
  const url = `http://localhost:5000/projects/${id}`;

  try {
    const response = await fetch(url, {
      method: 'DELETE',
    });

    if (!response.ok) {
      throw new Error('Failed to delete project');
    }

    // Clear the form
    deleteProjectId.value = '';
    
  } catch (error) {
    console.log('Error', error);
  }
}

const updateProject = async () => {

  const id = updateProjectId.value;
  const url = `http://localhost:5000/projects/${id}`;

  try {
    const response = await fetch(url, {
      method: 'PATCH',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(tempProject.value),
    });

    if (response.ok) {
      console.log('Updated project successfully');
    }

    if (!response.ok) {
      throw new Error('Failed to update project');
    }

    // Clear the form
    updateProjectId.value = '';
    tempProject.value.name = '';
    tempProject.value.description = '';
    
  } catch (error) {
    console.log('Error', error);
  }
}

</script>

<style scoped>

</style>