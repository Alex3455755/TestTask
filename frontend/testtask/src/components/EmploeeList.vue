<template>
  <div>
    <table border="1" style="border-collapse: collapse; width: 100%;">
      <thead>
        <tr>
          <th>Имя</th>
          <th>Фамилия</th>
          <th>Возраст</th>
          <th>Стаж</th>
          <th>Адрес</th>
          <th>Действие</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="emp in employees" :key="emp.firstName">
          <td>{{ emp.firstName }}</td>
          <td>{{ emp.lastName }}</td>
          <td>{{ emp.age }}</td>
          <td>{{ emp.experience }} года</td>
          <td>{{ emp.address }}</td>
          <td>
            <button @click="openEditModal(emp)">Редактировать</button>
          </td>
        </tr>
      </tbody>
    </table>
    
    <ModalEmploee
      v-if="showEditModal" 
      :employee="selectedEmployee"
      @close="closeEditModal"
      @update:employee="handleEmployeeUpdate"
    />
  </div>
</template>

<script>
import ModalEmploee from './ModalEmploee.vue';

export default {
  components: {
    ModalEmploee
  },
  data() {
    return {
      employees: [
        {
            id: 1,
          firstName: 'Ваня',
          lastName: 'Иванов',
          age: 20,
          experience: 3,
          address: 'Ленина 15',
        },
        {
            id: 2,
          firstName: 'Аня',
          lastName: 'Сидорова',
          age: 22,
          experience: 2,
          address: 'Старцева 7',
        }
      ],
      showEditModal: false,
      selectedEmployee: null
    };
  },
  methods: {
    openEditModal(employee) {
      this.selectedEmployee = { ...employee };
      this.showEditModal = true;
    },
    closeEditModal() {
      this.showEditModal = false;
      this.selectedEmployee = null;
    },
    handleEmployeeUpdate(updatedEmployee) {
      const index = this.employees.findIndex(emp => 
        emp.id === updatedEmployee.id
      );
      if (index !== -1) {
        
        this.employees[index] = updatedEmployee;

      }
      this.closeEditModal();
    }
  }
}
</script>
