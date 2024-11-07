<script>
import { ref, computed } from 'vue';

export default {
  name: "App",
  data() {
    return {
      newUser: {
        name: "",
        email: "",
        contactNumber: "",
        age: "",
        gender: "",
        birthdate: "",
        address: "",
      },
      users: [],
    };
  },
  computed: {
  
    isFormValid() {
      return (
        this.newUser.name &&
        this.newUser.email &&
        this.newUser.contactNumber &&
        this.newUser.age &&
        this.newUser.gender &&
        this.newUser.birthdate &&
        this.newUser.address
      );
    }
  },
  methods: {
    addUser() {
      if (this.isFormValid) {
        const newUser = {
          ...this.newUser,
          id: Date.now(),
        };
        this.users.push(newUser);
        this.resetForm();
      }
    },
    deleteUser(index) {
      this.users.splice(index, 1);
    },
    editUser(index) {
      const userToEdit = this.users[index];
      this.newUser = { ...userToEdit };
      this.deleteUser(index);
    },
    validateContactNumber(event) {
      const value = event.target.value.replace(/[^0-9]/g, ''); 
      this.newUser.contactNumber = value; 
    },
    autoFillBirthdate() {
      if (this.newUser.age && !isNaN(this.newUser.age)) {
        const currentYear = new Date().getFullYear();
        const birthYear = currentYear - this.newUser.age;
        this.newUser.birthdate = `${birthYear}-01-01`;
      }
    },
    resetForm() {
      this.newUser = {
        name: "",
        email: "",
        contactNumber: "",
        age: "",
        gender: "",
        birthdate: "",
        address: "",
      };
    }
  },
};

</script>

<template>
  <div id="app" class="container mt-5">
    <h1 class="header-title text-center mb-5">User Management</h1>
  </div>

  <div class="form-section w-75 mx-auto mb-4">
    <h2 class="h2">Create User</h2>
    
    <!-- Fullname input -->
    <div class="form-group mb-4">
      <input
        v-model="newUser.name"
        type="text"
        class="form-control"
        placeholder="Enter Fullname"
        required
      />
    </div>

    <div class="form-group mb-4">
      <input
        v-model="newUser.email"
        type="email"
        class="form-control"
        placeholder="Enter Email"
        required
      />
    </div>

    <div class="form-group mb-4">
      <input
        v-model="newUser.contactNumber"
        type="text"
        class="form-control"
        placeholder="Enter Contact Number"
        @input="validateContactNumber"
        required
      />
    </div>

    <div class="form-group mb-4">
      <input
        v-model="newUser.age"
        type="number"
        class="form-control"
        placeholder="Enter Age"
        @input="autoFillBirthdate"
        required
      />
    </div>

    <div class="form-group mb-4">
      <select v-model="newUser.gender" class="form-select" required>
        <option value="" disabled>Select Gender</option>
        <option value="Male">Male</option>
        <option value="Female">Female</option>
        <option value="Other">Other</option>
      </select>
    </div>

    <div class="form-group mb-4">
      <input
        v-model="newUser.birthdate"
        type="date"
        class="form-control"
        required
      />
    </div>

    <div class="form-group mb-4">
      <textarea
        v-model="newUser.address"
        class="form-control"
        placeholder="Enter Address"
        rows="3"
        required
      ></textarea>
    </div>

    <button
      class="btn w-100"
      :class="{
        'btn-primary': isFormValid,
        'btn-secondary': !isFormValid 
      }"
      @click="addUser"
      :disabled="!isFormValid"
    >
      <i class="bi bi-plus-circle-fill"></i> Add User
    </button>
  </div>

  <div class="table-section w-75 mx-auto mt-4">
    <h2 class="mb-3">User List</h2>
    <div class="table-responsive">
      <table class="table table-bordered table-striped">
        <thead>
          <tr>
            <th>Name</th>
            <th>Email</th>
            <th>Contact Number</th>
            <th>Age</th>
            <th>Gender</th>
            <th>Birthdate</th>
            <th>Address</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(user, index) in users" :key="user.id">
            <td>{{ user.name }}</td>
            <td>{{ user.email }}</td>
            <td>{{ user.contactNumber }}</td>
            <td>{{ user.age }}</td>
            <td>{{ user.gender }}</td>
            <td>{{ user.birthdate }}</td>
            <td>{{ user.address }}</td>
            <td>
              <button
                class="btn btn-success btn-sm"
                @click="editUser(index)"
              >
                <i class="bi bi-pencil-square"></i> Edit
              </button>
              <button
                class="btn btn-danger btn-sm ml-2"
                @click="deleteUser(index)"
              >
                <i class="bi bi-trash-fill"></i> Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>

  <div class="footer">
    <h6 class="header-title-h6"><p class="footer-text">Developed by: Mark Anthony Estopa</p></h6>
  </div>
</template>

<style scoped>

.container {
  max-width: 800px;
  margin: auto;
  padding: auto;
}

.header-title {
  font-size: 3rem;
  font-weight: bold;
  color: #333;
  text-transform: uppercase;
}

.form-section, .table-section {
  margin-bottom: 30px;
}

.table {
  width: 100%;
  margin-top: 20px;
  text-align: left;
}

.table th {
  background-color: #f8f9fa;
  text-align: center;
}

.table td {
  vertical-align: middle;
}

.table-responsive {
  max-height: 500px;
  overflow-y: auto;
}

input,
textarea,
select {
  padding: 10px;
  font-size: 1rem;
  border-radius: 8px;
}

.form-group {
  margin-bottom: 15px;
}

.btn {
  width: 100%;
}

.mx-auto {
  margin-left: auto;
  margin-right: auto;
}

h2 {
  margin-bottom: 15px;
}

button {
  width: 100%;
  margin-top: 10px;
}

h2 {
  text-align: center;
}
.header-title-h6 {
  background-color: black;
  text-align: center;
  font-weight: bold;
  color: #333;
  text-transform: uppercase;
  padding: 10px;
}
.footer-text {
  color: white;
}
</style>
