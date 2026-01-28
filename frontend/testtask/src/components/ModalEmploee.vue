<template>
  <div class="modal-overlay">
    <div class="modal-content" @click.stop>
      <h3>Редактировать сотрудника</h3>
      
      <form @submit.prevent="saveEmployee">
        <input type="hidden" v-model="formData.id">
        
        <div class="form-group">
          <label>Имя:</label>
          <input v-model="formData.firstName" required />
        </div>
        
        <div class="form-group">
          <label>Фамилия:</label>
          <input v-model="formData.lastName" required />
        </div>
        
        <div class="form-group">
          <label>Возраст:</label>
          <input v-model.number="formData.age" type="number" required />
        </div>
        
        <div class="form-group">
          <label>Стаж (лет):</label>
          <input v-model.number="formData.experience" type="number" required />
        </div>
        
        <div class="form-group">
          <label>Адрес:</label>
          <input v-model="formData.address" required />
        </div>
        
        <div class="modal-actions">
          <button type="button" @click="emitClose">Отмена</button>
          <button type="submit">Сохранить</button>
        </div>
      </form>
    </div>
  </div>
</template>


<script>
export default {
  name: 'ModalEmploee',
  props: {
    employee: {
      type: Object,
      required: true
    }
  },
  emits: ['close', 'update:employee'],
  data() {
    return {
      formData: { ...this.employee }
    }
  },
  methods: {
    emitClose() {
      this.$emit('close');
    },
    saveEmployee() {
      this.$emit('update:employee', { ...this.formData });
      this.$emit('close');
    }
  }
}
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}


.modal-content {
  background: white;
  padding: 2rem;
  border-radius: 8px;
  max-width: 500px;
  width: 90%;
  max-height: 80vh;
  overflow-y: auto;
}


.form-group {
  margin-bottom: 1rem;
}


.form-group label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: bold;
}


.form-group input {
  width: 100%;
  padding: 0.5rem;
  border: 1px solid #ddd;
  border-radius: 4px;
}


.modal-actions {
  display: flex;
  gap: 1rem;
  justify-content: flex-end;
  margin-top: 1.5rem;
}

.modal-actions button[type="submit"] {
  background: #6c757d;
  color: white;
}
</style>
