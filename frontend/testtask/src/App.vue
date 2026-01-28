<template>
  <EmploeeList />
</template>

<script>
import EmploeeList from './components/EmploeeList.vue';


export default {
  name: 'App',
  components: {
    EmploeeList
  }
}
</script>
<style>
button{
  background: #28a745;
  color: white;
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover{
  background: #6c757d;
}
#btnAdd{
  margin-top: 30px;
}
</style>