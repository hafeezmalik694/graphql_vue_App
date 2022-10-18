<template>
  <div>
  <h2>Artists List</h2>
  <table border='1' width='100%' style='border-collapse: collapse;'>
   <tr>
     <th>First Name</th>
     <th>Last Name</th>
     <th>Email</th>
     <th>Item Count</th>
     <th>Actions</th>
   </tr>
   <p v-if="error"> {{error}} </p>
   <p v-if="loading"> loading.......... </p>
   <template v-else>
   <tr v-for='artist in artists' v-bind:key="artist.id">
     <td>{{ artist.firstName }}</td>
     <td>{{ artist.lastName }}</td>
     <td>{{ artist.email }}</td>
     <td>{{ artist.itemsCount}}</td>
     <td>
      <input type="button" @click="selectArtist(artist)" value="Select">
      <input type="button" @click="deleteArtist(artist.id)" value="Delete">
     </td> 
   </tr>
  </template>
 </table>
</div>
</template>

<script setup>
import gql from 'graphql-tag';
import { useQuery } from '@vue/apollo-composable';
import { watchEffect, computed } from '@vue/runtime-core';

const ALL_ARTISTS = gql`
 query {
   artists{
     id
     email
     firstName
     lastName
     itemsCount
     items {
       id
       title
       description
     }
   }
 }
`;

const { result, loading, error } = useQuery(ALL_ARTISTS);
const artists = computed(() => result.value?.artists ?? []);
watchEffect(() => {
  console.log(artists.value);
})

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
