<template>
  <div>
    <!-- This is the top header controller btns -->
    <div class="row justify-content-center align-items-center g-2 my-3">
      <div class="col-4 text-end">
        <label v-if="showInProgress == false" for="showInProgress">In Available</label>
      </div>
      <div class="col-4 text-center">
        <label class="switch">
          <input id="showInProgress" v-model="showInProgress" type="checkbox" />
          <span class="slider round"></span>
        </label>
      </div>
      <div class="col-4 text-start">
        <label v-if="showInProgress == true" for="showInProgress">In Progress</label>
      </div>
    </div>

    <!-- Mission Card Component -->
    <div
      v-for="(item, index) in allMissions"
      :key="index"
      class="my-3 mx-3 pad-d card"
      style="padding-top: 0px; border: 0px"
    >
      <img
        class="card-img-top"
        src="../assets/img/test.jpg"
        alt="Title"
        style="border-radius: 16px 16px 0px 0px"
      />
      <div class="card-body">
        <h4 class="card-title">
          <h5>{{ item.missionID }}</h5>
        </h4>
        <p class="card-text">{{ item.description }}</p>
        <div v-if="showInProgress == false" class="text-end">
          <button type="button" class="btn primary-btn" @click="acceptMission(item.missionID)">
            <div>Accept</div>
          </button>
        </div>
        <div v-else class="">
          <form @submit.prevent="handleSubmit(item.missionID)">
            <div class="row justify-content-center align-items-center g-0">
              <div class="col-8"><input :id="item.missionID" type="text" name="" /></div>
              <div class="col-4 text-end">
                <input type="submit" value="Verify" class="btn primary-btn" />
              </div>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return { allMissions: '', showInProgress: false }
  },

  computed: {},
  watch: {
    showInProgress(status) {
      this.updateMissionsByStatus(status)
    }
  },
  async beforeMount() {
    // ### Dynamic Pull based on the userID (uncomment the btm)
    // const userID = localStorage.getItem('userID')
    // const response = await axios.get('http://127.0.0.1:5201/mission/{userID}')

    // ### Testing Code
    const response = await axios.get('http://127.0.0.1:5201/mission/2')
    this.allMissions = response.data.data.missions
  },
  methods: {
    async updateMissionsByStatus(status) {
      if (status == false) {
        const response = await axios.get('http://127.0.0.1:5201/mission/2')
        this.allMissions = response.data.data.missions
      } else {
        const response = await axios.get('http://127.0.0.1:5201/in_progress_mission/2')
        this.allMissions = response.data.data.missions
      }
    },
    async handleSubmit(missionID) {
      // const userID = localStorage.getItem('userID')
      const inputVal = document.getElementById(missionID).value
      const params = {
        missionID: inputVal,
        userID: 2,
        verification_code: '528819'
      }
      const response = await axios
        .post('http://127.0.0.1:5201/complete_mission', params)
        .then(() => {
          console.log('This is completed!')
        })
        .catch((err) => {
          console.error(err)
        })

      this.allMissions = response.data.data.missions
    },
    async acceptMission(missionID) {
      // const userID = localStorage.getItem('userID')
      const params = {
        missionID: missionID,
        userID: 2
      }
      const response = await axios
        .post('http://127.0.0.1:5201/accept_mission', params)
        .then(() => {
          console.log('This is completed!')
        })
        .catch((err) => {
          console.error(err)
        })
    }
  }
}
</script>
