<template>
  <div>
    <!-- ОСНОВНАЯ ТАБЛИЦА СОТРУДНИКОВ -->
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
        <!-- ЦИКЛ ПО СОТРУДНИКАМ - v-for создает строку для каждого сотрудника -->
        <tr v-for="emp in employees" :key="emp.firstName">
          <td>{{ emp.firstName }}</td>
          <td>{{ emp.lastName }}</td>
          <td>{{ emp.age }}</td>
          <td>{{ emp.experience }} года</td>
          <td>{{ emp.address }}</td>
          <!-- КНОПКА РЕДАКТИРОВАНИЯ - передает объект сотрудника в метод -->
          <td>
            <button @click="openEditModal(emp)">Редактировать</button>
          </td>
        </tr>
      </tbody>
    </table>
    
    <!-- КНОПКА ДОБАВЛЕНИЯ - открывает модалку для нового сотрудника -->
    <button id="btnAdd" @click="addEmploee">Добавить сотрудника</button>
    
    <!-- МОДАЛЬНОЕ ОКНО - показывается условно через v-if -->
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
      // МАССИВ СОТРУДНИКОВ - основное хранилище данных
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
      // ВЫБРАННЫЙ СОТРУДНИК - для редактирования/создания
      selectedEmployee: null,
      // ВИДИМОСТЬ МОДАЛКИ - true/false для показа/скрытия
      showEditModal: false
    };
  },
  methods: {
    // ДОБАВЛЕНИЕ НОВОГО - создает пустой объект для новой записи
    addEmploee() {
      this.selectedEmployee = {}; // ПУСТОЙ ОБЪЕКТ для новой записи
      this.showEditModal = true;
    },
    
    // РЕДАКТИРОВАНИЕ - создает КОПИЮ объекта для безопасного редактирования
    openEditModal(employee) {
      // {...employee} - поверхностное копирование (shallow copy)
      // Зачем? Изменения в модалке не затронут оригинал до сохранения
      this.selectedEmployee = { ...employee };
      this.showEditModal = true;
    },
    
    // ЗАКРЫТИЕ МОДАЛКИ - сбрасывает состояние
    closeEditModal() {
      this.showEditModal = false;
      this.selectedEmployee = null;
    },
    
    // САМЫЙ СЛОЖНЫЙ МЕТОД - сохранение изменений
    handleEmployeeUpdate(updatedEmployee) {
      // НАХОДИМ ПО ID - findIndex возвращает индекс или -1
      const index = this.employees.findIndex(emp => 
        emp.id === updatedEmployee.id  // СРАВНИВАЕМ ID
      );
      
      // РЕДАКТИРОВАНИЕ СУЩЕСТВУЮЩЕГО
      if (index !== -1) {
        this.employees[index] = updatedEmployee;
      } 
      // ДОБАВЛЕНИЕ НОВОГО (нет ID или не найден)
      else {
        // ВНИМАНИЕ: для новых сотрудников нужно добавить ID!
        // Можно использовать Date.now() или счетчик
        updatedEmployee.id = Date.now();
        console.log(updatedEmployee.id);
        this.employees.push(updatedEmployee);
      }
      
      this.closeEditModal(); // ОЧИСТКА после сохранения
    }
  }
}
</script>
