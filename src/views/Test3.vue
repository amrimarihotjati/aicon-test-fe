<template>
  <div class="container" style="height: 100vh;">
    <div 
      class="row rounded-3" 
      style="margin-top: 100px; background-color: #edfaff; padding: 20px;"
    >
      <div class="col-4">
        <h1 class="text-left fw-bold fs-2 mb-3">Report Detail</h1>
        <div class="d-flex align-items-center mb-3">
          <input type="text" class="form-control me-2" v-model="filter" placeholder="Enter keyword" style="height: 35px; width: 50%;">
          <button class="btn btn-primary" @click="applyFilter" style="height: 35px;">Search</button>
        </div>
        <!-- Tabs -->
        <ul class="nav nav-tabs" id="myTab" role="tablist" style="gap: 5px;">
          <li class="nav-item" role="presentation">
            <button class="nav-link active" id="general-tab" data-bs-toggle="tab" data-bs-target="#general" type="button" role="tab" aria-controls="general" aria-selected="true" style="width: 140px;">Pages</button>
          </li>
          <li class="nav-item" role="presentation">
            <button class="nav-link" id="economic-tab" data-bs-toggle="tab" data-bs-target="#economic" type="button" role="tab" aria-controls="economic" aria-selected="false" style="width: 140px;">Result</button>
          </li>
        </ul>
        <div class="tab-content" id="myTabContent">
          <div class="tab-pane fade show active" id="general" role="tabpanel" aria-labelledby="general-tab">
            <!-- Button with Icons -->
            <button class="btn mt-2 d-flex align-items-center justify-content-between" @click="toggleVisibility" style="width: 250px; gap: 10px; background-color: #f03736; color: white;">
              <i class="fas fa-book-open"></i> <!-- Main Icon -->
              General
              <i class="fas fa-chevron-down" v-if="!isVisible"></i> <!-- Arrow Icon for collapsed state -->
              <i class="fas fa-chevron-up" v-if="isVisible"></i> <!-- Arrow Icon for expanded state -->
            </button>
            <button class="btn mt-2 d-flex align-items-center justify-content-between" @click="toggleVisibility" style="width: 250px; gap: 10px; background-color: #2461d5; color: white;">
              <i class="fas fa-book-open"></i> <!-- Main Icon -->
              Economic
              <i class="fas fa-chevron-down" v-if="!isVisible"></i> <!-- Arrow Icon for collapsed state -->
              <i class="fas fa-chevron-up" v-if="isVisible"></i> <!-- Arrow Icon for expanded state -->
            </button>
            <button class="btn mt-2 d-flex align-items-center justify-content-between" @click="toggleVisibility" style="width: 250px; gap: 10px; background-color: #23a536; color: white;">
              <i class="fas fa-book-open"></i> <!-- Main Icon -->
              Environment
              <i class="fas fa-chevron-down" v-if="!isVisible"></i> <!-- Arrow Icon for collapsed state -->
              <i class="fas fa-chevron-up" v-if="isVisible"></i> <!-- Arrow Icon for expanded state -->
            </button>
            <button class="btn mt-2 d-flex align-items-center justify-content-between" @click="toggleVisibility" style="width: 250px; gap: 10px; background-color: #e89c42; color: white;">
              <i class="fas fa-book-open"></i> <!-- Main Icon -->
              Social
              <i class="fas fa-chevron-down" v-if="!isVisible"></i> <!-- Arrow Icon for collapsed state -->
              <i class="fas fa-chevron-up" v-if="isVisible"></i> <!-- Arrow Icon for expanded state -->
            </button>
            <div v-if="isVisible">
              <p class="mt-2">Detailed page settings and options here.</p>
            </div>
          </div>
          <div class="tab-pane fade" id="economic" role="tabpanel" aria-labelledby="economic-tab">
            <p>Economic data and analytics.</p>
          </div>
        </div>
      </div>
      <div class="col-8">
        <!-- Dropdown menus -->
        <div class="mb-4">
          <div class="row justify-content-between">
            <div class="col-4">
              <label for="reportType" class="form-label">Standard</label>
              <select id="reportType" class="form-select" v-model="selectedReportType" style="width: 100%;">
                <option disabled value="">Select Standard</option>
                <option>GRI</option>
              </select>
            </div>
            <div class="col-4">
              <label for="year" class="form-label">View Report As</label>
              <select id="year" class="form-select" v-model="selectedYear" style="width: 100%;">
                <option disabled value=""> Select</option>
                <option>Kantor Pusat</option>
              </select>
            </div>
            <div class="col-4">
              <label for="region" class="form-label">Region</label>
              <select id="region" class="form-select" v-model="selectedRegion" style="width: 100%;">
                <option disabled value="">Select region</option>
                <option>Pusat Timur</option>
              </select>
            </div>
          </div>
        </div>
        <!-- Large Green Box -->
        <div class="bg-success text-white p-3 mb-3 rounded">
          <h4>GRI - 300</h4>
          <p>Penggunaan Energi Dalam Organisasi</p>
        </div>
        <!-- Data Table -->
        <div class="table-responsive">
          <table class="table">
            <thead class="table-light">
              <tr>
                <th>#</th>
                <th>Category</th>
                <th>Usage</th>
                <th>Cost</th>
                <th>Savings</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, index) in reportData" :key="index">
                <td>{{ index + 1 }}</td>
                <td>{{ item.category }}</td>
                <td>{{ item.usage }}</td>
                <td>{{ item.cost }}</td>
                <td>{{ item.savings }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      filter: '',
      isVisible: true,
      selectedReportType: '',
      selectedYear: '',
      selectedRegion: '',
      reportData: [
        { category: 'Electricity', usage: '1,500 kWh', cost: '$300', savings: '$50' },
        { category: 'Water', usage: '120 m³', cost: '$100', savings: '$20' },
        { category: 'Gas', usage: '800 m³', cost: '$400', savings: '$60' }
      ]
    };
  },
  methods: {
    applyFilter() {
      console.log('Filter applied:', this.filter);
    },
    toggleVisibility() {
      this.isVisible = !this.isVisible;
    }
  }
}
</script>

<style scoped>
  .nav-tabs .nav-link {
    color: #495057; /* Bootstrap default for non-active tabs */
    background-color: #cde3f4; /* Light blue background for tabs */
  }
  .nav-tabs .nav-link.active {
    color: #ffffff; /* White text for active tab */
    background-color: #166eab; /* Dark blue background for active tab */
  }
</style>
