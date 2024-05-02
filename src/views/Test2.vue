<template>
  <div class="container" style="height: 100vh;">
    <div class="row" style="margin-top: 100px;">
      <h1 class="text-center fw-bold fs-1 mb-5">Branch List</h1>
      <div class="col-md-8">
        <div class="d-flex align-items-center">
          <input type="text" class="form-control me-2" v-model="filter" placeholder="Enter keyword" style="height: 35px; width: 50%;">
          <button class="btn btn-primary me-2" @click="applyFilter" style="height: 35px;">Search</button>
        </div>
      </div>
    </div>

    <!-- Loading Spinner -->
    <div class="row justify-content-center mt-4" v-if="isLoading">
      <div class="spinner-border text-primary" role="status">
        <span class="visually-hidden">Loading...</span>
      </div>
    </div>

    <!-- Table -->
    <div class="row mt-4" v-if="!isLoading">
      <div class="col-md">
        <table class="table table-striped table-hover">
          <thead class="bg-primary bg-opacity-75 text-white">
            <tr>
              <th @click="sort('value')">ID <img :src="idSortIcon" alt="Sort Icon"></th>
              <th @click="sort('text')">Branch <img :src="branchSortIcon" alt="Sort Icon"></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(branch, index) in paginatedBranches" :key="index">
              <td>{{ branch.value }}</td>
              <td>{{ branch.text }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <!-- Pagination -->
    <div class="row mt-4" v-if="!isLoading">
      <div class="col-md">
        <nav aria-label="Page navigation example">
          <ul class="pagination justify-content-center">
            <li class="page-item" :class="{ disabled: currentPage === 1 }">
              <button class="page-link" @click="prevPage" :disabled="currentPage === 1">Previous</button>
            </li>
            <li class="page-item" v-for="pageNumber in visiblePageNumbers" :key="pageNumber" :class="{ active: currentPage === pageNumber }">
              <button class="page-link" @click="goToPage(pageNumber)">{{ pageNumber }}</button>
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
import { ref } from 'vue';
import sortup from '../assets/sortup.svg';
import sortdown from '../assets/sortdown.svg';

export default {
  data() {
    return {
      branches: [],
      filter: '',
      currentPage: 1,
      pageSize: 10,
      idSortOrder: 'asc',
      branchSortOrder: 'asc',
      isLoading: false // Tambahkan variabel isLoading
    };
  },
  computed: {
    filteredBranches() {
      return this.branches.filter(branch => {
        return (
          branch.text.toLowerCase().includes(this.filter.toLowerCase()) ||
          branch.value.toString().toLowerCase().includes(this.filter.toLowerCase())
        );
      });
    },
    paginatedBranches() {
      const startIndex = (this.currentPage - 1) * this.pageSize;
      const endIndex = startIndex + this.pageSize;
      return this.filteredBranches.slice(startIndex, endIndex);
    },
    totalPages() {
      return Math.ceil(this.filteredBranches.length / this.pageSize);
    },
    visiblePageNumbers() {
      const totalVisiblePages = 5;
      const startPage = Math.max(1, this.currentPage - Math.floor(totalVisiblePages / 2));
      const endPage = Math.min(this.totalPages, startPage + totalVisiblePages - 1);
      return Array.from({ length: endPage - startPage + 1 }, (_, index) => startPage + index);
    },
    idSortIcon() {
      return this.idSortOrder === 'asc' ? sortup : sortdown;
    },
    branchSortIcon() {
      return this.branchSortOrder === 'asc' ? sortup : sortdown;
    },
  },
  methods: {
    fetchData() {
      this.isLoading = true; // Set isLoading menjadi true saat memuat data
      fetch('https://private-46841d-dummybljr.apiary-mock.com/incentive_api/api/v1/dropdown_branch')
        .then(response => response.json())
        .then(data => {
          this.branches = data.data;
          this.isLoading = false; // Set isLoading menjadi false setelah data dimuat
        })
        .catch(error => {
          console.error('Error fetching data:', error);
          this.isLoading = false; // Set isLoading menjadi false jika terjadi kesalahan
        });
    },
    applyFilter() {
      // Tidak perlu melakukan apa-apa di sini karena pencarian dilakukan secara real-time menggunakan computed property
    },
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },
    goToPage(page) {
      this.currentPage = page;
    },
    sort(column) {
      if (column === 'value') {
        this.branches.sort((a, b) => {
          if (this.idSortOrder === 'asc') {
            return a.value - b.value;
          } else {
            return b.value - a.value;
          }
        });
        this.idSortOrder = this.idSortOrder === 'asc' ? 'desc' : 'asc';
      } else if (column === 'text') {
        this.branches.sort((a, b) => {
          if (this.branchSortOrder === 'asc') {
            return a.text.localeCompare(b.text);
          } else {
            return b.text.localeCompare(a.text);
          }
        });
        this.branchSortOrder = this.branchSortOrder === 'asc' ? 'desc' : 'asc';
      }
    },
  },
  mounted() {
    this.fetchData();
  },
};
</script>
