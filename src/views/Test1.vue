<template>
  <div class="container" style="height: 100vh;">
    <div class="row" style="margin-top: 100px;">
      <h1 class="text-center fw-bold fs-1 mb-5">User List</h1>
      <div class="col-md-8">
        <div class="d-flex align-items-center">
          <input type="text" class="form-control me-2" v-model="filter" placeholder="Enter keyword" style="height: 35px; width: 50%;">
          <button class="btn btn-primary me-2" @click="applyFilter" style="height: 35px;">Filter</button>
        </div>
      </div>
      <div class="col-md-4 text-end">
        <button type="button" class="btn btn-success" data-bs-toggle="modal" data-bs-target="#exampleModal" style="height: 35px;">Create User</button>
      </div>
    </div>

    <!-- Modal -->
    <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5" id="exampleModalLabel">Create User</h1>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <form @submit.prevent="saveUser">
              <div class="mb-3">
                <label for="name" class="form-label">Name:</label>
                <input type="text" class="form-control" id="name" v-model="newUser.name" required>
              </div>
              <div class="mb-3">
                <label for="email" class="form-label">Email:</label>
                <input type="email" class="form-control" id="email" v-model="newUser.email" required>
              </div>
              <div class="mb-3">
                <label for="address" class="form-label">Address:</label>
                <input type="text" class="form-control" id="address" v-model="newUser.address" required>
              </div>
              <div class="mb-3">
                <label for="ktpNumber" class="form-label">KTP Number:</label>
                <input type="text" class="form-control" id="ktpNumber" v-model="newUser.ktpNumber" @change="handleKTPNumberChange" required>
              </div>
              <div class="mb-3">
                <label for="gender" class="form-label">Gender:</label>
                <input type="text" class="form-control" id="gender" v-model="newUser.gender" required readonly>
              </div>
              <div class="mb-3">
                <label for="birthdate" class="form-label">Birthdate:</label>
                <input type="text" class="form-control" id="birthdate" v-model="newUser.birthdate" required readonly>
              </div>
              <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                <button type="submit" class="btn btn-primary">Save</button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>

    <!-- Table -->
    <div class="row mt-4">
      <div class="col-md">
        <table class="table table-striped table-hover">
          <thead class="bg-primary bg-opacity-75 text-white">
            <tr>
              <th @click="sortBy('id')">ID <img :src="sortIcon('id')"></th>
              <th @click="sortBy('name')">Name <img :src="sortIcon('name')"></th>
              <th @click="sortBy('email')">Email <img :src="sortIcon('email')"></th>
              <th @click="sortBy('address')">Address <img :src="sortIcon('address')"></th>
              <th @click="sortBy('ktpNumber')">KTP Number <img :src="sortIcon('ktpNumber')"></th>
              <th @click="sortBy('gender')">Gender <img :src="sortIcon('gender')"></th>
              <th @click="sortBy('birthdate')">Birthdate <img :src="sortIcon('birthdate')"></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(user, index) in paginatedUsers" :key="index">
              <td>{{ user.id }}</td>
              <td>{{ user.name }}</td>
              <td>{{ user.email }}</td>
              <td>{{ user.address }}</td>
              <td>{{ user.ktpNumber }}</td>
              <td>{{ user.gender }}</td>
              <td>{{ user.birthdate }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <!-- Pagination -->
    <div class="row mt-4">
      <div class="col-md">
        <nav aria-label="Page navigation example">
          <ul class="pagination justify-content-center">
            <li class="page-item" :class="{ disabled: currentPage === 1 }">
              <button class="page-link" @click="prevPage" :disabled="currentPage === 1">Previous</button>
            </li>
            <li class="page-item" v-for="page in totalPages" :key="page" :class="{ active: currentPage === page }">
              <button class="page-link" @click="goToPage(page)">{{ page }}</button>
            </li>
            <li class="page-item" :class="{ disabled: currentPage === totalPages }">
              <button class="page-link" @click="nextPage" :disabled="currentPage === totalPages">Next</button>
            </li>
          </ul>
        </nav>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue';
import { usersData } from '../../fake-api/users';
import sortup from '../assets/sortup.svg';
import sortdown from '../assets/sortdown.svg';

export default {
  name: 'UserManagement',
  setup() {
    const newUser = ref({
      id: '', // Tambahkan properti ID ke newUser
      name: '',
      email: '',
      address: '',
      ktpNumber: '',
      gender: '',
      birthdate: ''
    });

    const users = ref(usersData);

    // Properti untuk filtering
    const filter = ref('');

    // Properti untuk sorting
    const sortColumn = ref('');
    const sortOrder = ref(1); // 1 untuk ascending, -1 untuk descending

    // Logika paginasi
    const usersPerPage = 5;
    const currentPage = ref(1);

    const paginatedUsers = computed(() => {
      // Filtering
      let filteredUsers = users.value.filter(user => {
        return (
          user.id.toString().includes(filter.value) ||
          user.name.toLowerCase().includes(filter.value.toLowerCase()) ||
          user.email.toLowerCase().includes(filter.value.toLowerCase()) ||
          user.address.toLowerCase().includes(filter.value.toLowerCase()) ||
          user.ktpNumber.toLowerCase().includes(filter.value.toLowerCase()) ||
          user.gender.toLowerCase().includes(filter.value.toLowerCase()) ||
          user.birthdate.toLowerCase().includes(filter.value.toLowerCase())
        );
      });

      // Sorting
      if (sortColumn.value) {
        filteredUsers = filteredUsers.sort((a, b) => {
          if (sortColumn.value === 'id') {
            return (a.id - b.id) * sortOrder.value;
          } else {
            const fieldA = a[sortColumn.value].toLowerCase();
            const fieldB = b[sortColumn.value].toLowerCase();
            if (fieldA < fieldB) {
              return -1 * sortOrder.value;
            }
            if (fieldA > fieldB) {
              return 1 * sortOrder.value;
            }
            return 0;
          }
        });
      }

      const startIndex = (currentPage.value - 1) * usersPerPage;
      const endIndex = startIndex + usersPerPage;
      return filteredUsers.slice(startIndex, endIndex);
    });

    const totalPages = computed(() => Math.ceil(users.value.length / usersPerPage));

    const goToPage = (page) => {
      currentPage.value = page;
    };

    const nextPage = () => {
      if (currentPage.value < totalPages.value) {
        currentPage.value++;
      }
    };

    const prevPage = () => {
      if (currentPage.value > 1) {
        currentPage.value--;
      }
    };

    // Handle perubahan pada nomor KTP
    const handleKTPNumberChange = () => {
      const ktpNumber = newUser.value.ktpNumber;
      const wilayah = ktpNumber.substring(0, 6);
      let tanggalLahir = ktpNumber.substring(6, 12);
      const bulanNames = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];
      let gender = 'Male';
      let day = parseInt(tanggalLahir.substring(0, 2));
      const month = parseInt(tanggalLahir.substring(2, 4));
      let year = parseInt(tanggalLahir.substring(4, 6));

      // Jika tanggal lahir perempuan, kurangi 40 dari tanggal lahir
      if (parseInt(tanggalLahir.substring(0, 2)) >= 40) {
        gender = 'Female';
        day -= 40;
      }

      // Format tanggal lahir
      tanggalLahir = `${(day < 10 ? '0' : '') + day} ${bulanNames[month - 1]} ${(year < 10 ? '20' : '19')}${year}`;

      newUser.value.gender = gender;
      newUser.value.birthdate = tanggalLahir;
    };

    // Fungsi untuk menyimpan pengguna baru dan menutup modal
    const saveUser = () => {
      const lastUserId = users.value.length > 0 ? users.value[users.value.length - 1].id : 0;
      const newId = lastUserId + 1;
      const user = {
        id: newId,
        name: newUser.value.name,
        email: newUser.value.email,
        address: newUser.value.address,
        ktpNumber: newUser.value.ktpNumber,
        gender: newUser.value.gender,
        birthdate: newUser.value.birthdate
      };
      users.value.push(user);
      clearForm();
      closeModal();
    };

    const closeModal = () => {
      const modal = new bootstrap.Modal(document.getElementById('exampleModal'));
      modal.hide();
    };

    const clearForm = () => {
      newUser.value.id = '';
      newUser.value.name = '';
      newUser.value.email = '';
      newUser.value.address = '';
      newUser.value.ktpNumber = '';
      newUser.value.gender = '';
      newUser.value.birthdate = '';
    };

    const sortIcon = (column) => {
      if (sortColumn.value === column) {
        return sortOrder.value === 1 ? sortup : sortdown;
      }
      return '';
    };

    const sortBy = (column) => {
      if (column === 'asc') {
        sortOrder.value = 1;
      } else if (column === 'desc') {
        sortOrder.value = -1;
      } else {
        if (sortColumn.value === column) {
          sortOrder.value *= -1;
        } else {
          sortColumn.value = column;
          sortOrder.value = 1;
        }
      }
    };

    const applyFilter = () => {
      currentPage.value = 1;
    };

    return {
      newUser,
      users,
      currentPage,
      paginatedUsers,
      totalPages,
      filter,
      handleKTPNumberChange,
      saveUser,
      clearForm,
      goToPage,
      nextPage,
      prevPage,
      sortBy,
      sortIcon,
      applyFilter
    };
  }
};
</script>
