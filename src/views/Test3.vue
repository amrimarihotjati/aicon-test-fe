<template>
  <div class="container-fluid">
    <!-- Header Section -->
    <div class="row bg-primary text-white align-items-center px-4 py-2">
      <div class="col-auto">
        <h2>Report Detail</h2>
      </div>
      <div class="col-auto ms-auto">
        <select class="form-select">
          <option>2021</option>
          <option>2022</option>
        </select>
      </div>
      <div class="col-auto">
        <select class="form-select">
          <option>Period 1</option>
          <option>Period 2</option>
        </select>
      </div>
    </div>

    <!-- Sidebar Navigation -->
    <div class="row flex-grow-1">
      <div class="col-md-2 p-0">
        <div class="bg-dark vh-100">
          <ul class="nav flex-column text-white">
            <li class="nav-item">
              <a href="#" class="nav-link active bg-primary">General</a>
            </li>
            <li class="nav-item">
              <a href="#" class="nav-link text-white">Economic</a>
            </li>
            <li class="nav-item">
              <a href="#" class="nav-link text-white">Environment</a>
            </li>
            <li class="nav-item">
              <a href="#" class="nav-link text-white">Social</a>
            </li>
          </ul>
        </div>
      </div>

      <!-- Main Content Area -->
      <div class="col-md-10">
        <!-- Tab Navigation -->
        <ul class="nav nav-tabs">
          <li class="nav-item">
            <a class="nav-link active" data-bs-toggle="tab" href="#tab1">GRI-300 Environment</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" data-bs-toggle="tab" href="#tab2">GRI-302 Energy</a>
          </li>
        </ul>

        <!-- Tab Content -->
        <div class="tab-content p-3">
          <div class="tab-pane fade show active" id="tab1">
            <h4>Energy Usage in Organization</h4>
            <div class="table-responsive">
              <table class="table">
                <thead>
                  <tr>
                    <th>Description</th>
                    <th>Unit</th>
                    <th>2021</th>
                    <th>2022</th>
                    <th>Total</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <td>Water</td>
                    <td>Joule</td>
                    <td>1,840</td>
                    <td>2,300</td>
                    <td>4,140</td>
                  </tr>
                  <tr>
                    <td>Electricity</td>
                    <td>Joule</td>
                    <td>18,970</td>
                    <td>25,040</td>
                    <td>44,010</td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
          <div class="tab-pane fade" id="tab2">
            <!-- Content for GRI-302 Energy -->
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.nav-item a.nav-link {
  color: white;
}

.nav-item a.nav-link.active {
  background-color: #007bff; /* Bootstrap primary color */
}

.table th, .table td {
  vertical-align: middle;
}

/* Additional styles to closely match the screenshot */
</style>
