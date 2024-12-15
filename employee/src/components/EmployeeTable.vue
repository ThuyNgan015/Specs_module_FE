<template>
  <div>
    <h1>Danh sách nhân viên</h1>
    <button @click="showAddModal">+ Thêm Mới</button>

    <div v-if="showModal">
      <h3>{{ isEditMode ? 'Cập nhật Nhân viên' : 'Thêm Nhân viên mới' }}</h3>
      <form @submit.prevent="isEditMode ? updateEmployee() : addEmployee()">
        <label for="name">Tên:</label>
        <input id="name" v-model="form.name" required />

        <label for="birthDate">Ngày sinh:</label>
        <input id="birthDate" type="date" v-model="form.birthDate" required />

        <label for="gender">Giới tính:</label>
        <select id="gender" v-model="form.gender" required>
          <option value="Nam">Nam</option>
          <option value="Nữ">Nữ</option>
        </select>

        <label for="salary">Lương:</label>
        <input id="salary" type="number" v-model="form.salary" required />

        <button type="submit">{{ isEditMode ? 'Lưu' : 'Thêm' }}</button>
        <button type="button" @click="closeModal">Hủy</button>
      </form>
    </div>

    <table>
      <thead>
        <tr>
          <th>STT</th>
          <th>Tên</th>
          <th>Ngày sinh</th>
          <th>Giới tính</th>
          <th>Lương</th>
          <th>Thao tác</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(employee, index) in employees" :key="employee.id">
          <td>{{ index + 1 }}</td>
          <td>{{ employee.name }}</td>
          <td>{{ employee.birthDate }}</td>
          <td>{{ employee.gender }}</td>
          <td>{{ formatSalary(employee.salary) }}</td>
          <td>
            <button @click="showEditModal(employee)">Cập nhật</button>
            <button @click="deleteEmployee(employee.id)">Xóa</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      employees: [],
      showModal: false,
      isEditMode: false,
      form: {
        id: null,
        name: '',
        birthDate: '',
        gender: '',
        salary: null,
      },
    };
  },
  methods: {
    fetchEmployees() {
      axios.get('http://localhost:8080/employees')
        .then(response => {
          this.employees = response.data;
        })
        .catch(error => console.error(error));
    },
    addEmployee() {
      axios.post('http://localhost:8080/employees', this.form)
        .then(() => {
          this.fetchEmployees();
          this.closeModal();
        })
        .catch(error => console.error(error));
    },
    updateEmployee() {
      axios.put(`http://localhost:8080/employees/${this.form.id}`, this.form)
        .then(() => {
          this.fetchEmployees();
          this.closeModal();
        })
        .catch(error => console.error(error));
    },
    deleteEmployee(id) {
      axios.delete(`http://localhost:8080/employees/${id}`)
        .then(() => this.fetchEmployees())
        .catch(error => console.error(error));
    },
    showAddModal() {
      this.isEditMode = false;
      this.form = { id: null, name: '', birthDate: '', gender: '', salary: null };
      this.showModal = true;
    },
    showEditModal(employee) {
      this.isEditMode = true;
      this.form = { ...employee };
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
    },
    formatSalary(salary) {
      return new Intl.NumberFormat('vi-VN', { style: 'currency', currency: 'VND' }).format(salary);
    },
  },
  mounted() {
    this.fetchEmployees();
  },
};
</script>

<style>
body {
  font-family: 'Arial', sans-serif;
  background-color: #f9f9f9;
  color: #333;
  margin: 0;
  padding: 20px;
}

h1 {
  text-align: center;
  color: #4CAF50;
  font-size: 28px;
  margin-bottom: 20px;
}

button {
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  padding: 10px 15px;
  font-size: 14px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #45a049;
}

button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

form {
  background-color: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  max-width: 400px;
  margin: 20px auto;
}

form h3 {
  margin-bottom: 15px;
  color: #333;
  text-align: center;
}

form label {
  display: block;
  margin: 10px 0 5px;
  font-weight: bold;
  font-size: 14px;
}

form input, form select {
  width: 100%;
  padding: 8px;
  font-size: 14px;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin-bottom: 15px;
  box-sizing: border-box;
}

form input:focus, form select:focus {
  border-color: #4CAF50;
  outline: none;
  box-shadow: 0 0 5px rgba(76, 175, 80, 0.5);
}

form button {
  width: 48%;
  margin-right: 2%;
}

form button:last-child {
  margin-right: 0;
  background-color: #f44336;
}

form button:last-child:hover {
  background-color: #e53935;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
  background-color: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

table th {
  background-color: #4CAF50;
  color: white;
  padding: 10px;
  text-align: left;
  font-size: 14px;
}

table td {
  padding: 10px;
  border-bottom: 1px solid #ddd;
  font-size: 14px;
}

table tr:hover {
  background-color: #f1f1f1;
}

table button {
  font-size: 12px;
  padding: 6px 10px;
  margin-right: 5px;
}

table button:last-child {
  margin-right: 0;
  background-color: #f44336;
}

table button:last-child:hover {
  background-color: #e53935;
}

[hidden] {
  display: none;
}

@media (max-width: 768px) {
  table {
    font-size: 12px;
  }

  form {
    padding: 15px;
  }

  form button {
    width: 100%;
    margin: 5px 0;
  }
}

</style>
