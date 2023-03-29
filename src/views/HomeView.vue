<template>
  <div>
    <!-- This is the top header controller btns -->
    <div class="row g-2 text-center">
      <div class="col-1" />
      <div class="col-5">
        <button type="button" class="btn btn-primary" data-bs-toggle="button" aria-pressed="true">
          Available
        </button>
      </div>
      <div class="col-5">
        <button type="button" class="btn btn-primary" data-bs-toggle="button" aria-pressed="false">
          In Progress
        </button>
      </div>
      <div class="col-1" />
    </div>

    <!-- Mission Card Component -->
    <div
      v-for="(item, index) in allMissions"
      :key="index"
      class="my-3 mx-3 py-3 pad-d card text-white"
    >
      <img class="card-img-top" src="../assets/img/test.jpg" alt="Title" />
      <div class="card-body">
        <h4 class="card-title">
          <h5>{{ item.missionID }}</h5>
        </h4>
        <p class="card-text">{{ item.description }}</p>
        <button type="button" class="btn primary-btn" data-bs-toggle="button" aria-pressed="false">
          Accept
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return { allMissions: '' }
  },

  computed: {},
  async beforeMount() {
    // ### Dynamic Pull based on the userID (uncomment the btm)
    // const userID = localStorage.getItem('userID')
    // const response = await axios.get('http://127.0.0.1:5201/mission/{userID}')

    // ### Testing Code
    const response = await axios.get('http://127.0.0.1:5201/mission/2')
    this.allMissions = response.data.data.missions
  },
  methods: {}
}
</script>
