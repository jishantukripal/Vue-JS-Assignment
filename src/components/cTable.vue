<template>
    <div>
      <table class="custom-table">
        <thead>
          <tr>
            <th @click="sortData('slno')">Sl. No.</th>
            <th @click="sortData('name')">Name</th>
            <th @click="sortData('dob')">DOB</th>
            <th @click="sortData('email')">Email</th>
            <th @click="sortData('location')">Location</th>
            <th @click="sortData('phone')">Phone</th>
            <th @click="sortData('picture')">Picture</th>
          </tr>
        </thead>
        <tbody>
          <TableRow v-for="item in flattenedUserData" :key="item.slno" :rowData="item">
            <template #default="slotProps">
              <TableData :data="slotProps.rowData.slno">{{ slotProps.rowData.slno }}</TableData>
              <TableData :data="slotProps.rowData.name">{{ slotProps.rowData.name }}</TableData>
              <TableData :data="slotProps.rowData.dob">{{ slotProps.rowData.dob }}</TableData>
              <TableData :data="slotProps.rowData.email">{{ slotProps.rowData.email }}</TableData>
              <TableData :data="slotProps.rowData.location">{{ slotProps.rowData.location }}</TableData>
              <TableData :data="slotProps.rowData.phone">{{ slotProps.rowData.phone }}</TableData>
              <TableData :data="slotProps.rowData.picture">
                <img :src="slotProps.rowData.picture" alt="User">
              </TableData>
            </template>
          </TableRow>
        </tbody>
      </table>
    </div>
</template>
  
<script setup>
  import { ref, onMounted } from 'vue';
  import TableRow from './TableRow.vue';
  import TableData from './TableData.vue';
  
  const users = ref([]);
  const flattenedUserData = ref([]);
  
  onMounted(async () => {
    try {
      const response = await fetch('https://randomuser.me/api/?results=50');
      const data = await response.json();
      users.value = data.results;
      flattenUserData();
      sortData('slno_desc');
    } catch (error) {
      console.error('Error fetching data:', error);
    }
  });
  
  const flattenUserData = () => {
    flattenedUserData.value = users.value.map((user, index) => ({
      slno: index + 1,
      name: `${user.name.first} ${user.name.last}`,
      dob: user.dob.date,
      email: user.email,
      location: `${user.location.street.number} ${user.location.street.name}, ${user.location.city}, ${user.location.state}, ${user.location.country}`,
      phone: user.phone,
      picture: user.picture.thumbnail,
    }));
  };
  
  const sortData = (key) => {
    flattenedUserData.value.sort((a, b) => {
      const order = key.endsWith('_desc') ? -1 : 1;
      const field = key.replace('_desc', '').replace('_asc', '');
      return order * (a[field] > b[field] ? 1 : -1);
    });
  };
</script>
  
<style scoped>
  .custom-table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
  }
  
  .custom-table th,
  .custom-table td {
    border: 1px solid #ddd;
    padding: 8px;
    text-align: left;
  }
  
  .custom-table th {
    background-color: #f2f2f2;
    cursor: pointer;
  }
  
  .custom-table img {
    max-width: 50px;
    max-height: 50px;
  }
</style>
  