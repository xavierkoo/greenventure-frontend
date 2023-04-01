<template>
  <div class="background-radial-gradient">
    <div class="row g-0">
      <div class="col-1"></div>
      <div class="col pt-3" style="color: white">
        <h4>Welcome back!</h4>
        Time to pick a mission and start!
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
      </div>
      <div class="col-1"></div>
    </div>
    <div class="row g-0 mt-3">
      <div class="col-1"></div>
      <div class="col">
        <div class="row g-0">
          <div v-for="(item, index) in allMissions" :key="index" class="col-sm-6 col-lg-4">
            <!-- Mission Card Component -->
            <div class="my-3 mx-3 pad-d card" style="padding-top: 0px; border: 0px">
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
                  <button
                    type="button"
                    class="btn primary-btn"
                    @click="acceptMission(item.missionID)"
                  >
                    <div>Accept</div>
                  </button>
                </div>
                <div v-else class="">
                  <form @submit.prevent="handleSubmit(item.missionID)">
                    <div class="row justify-content-center align-items-center g-0">
                      <div class="col-12 col-md-8">
                        <input
                          :id="item.missionID"
                          type="text"
                          name=""
                          style="width: 100%; border-radius: 8px; border: 1px grey"
                        />
                      </div>
                      <div class="col-12 mt-2 mt-md-0 col-md-4 text-end">
                        <input type="submit" value="Verify" class="btn primary-btn" />
                      </div>
                    </div>
                  </form>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="col-1"></div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return { allMissions: '', showInProgress: false, userID: '' }
  },

  computed: {},
  watch: {
    showInProgress(status) {
      this.updateMissionsByStatus(status)
    }
  },
  async beforeMount() {
    // Get userId from localStorage
    this.userID = localStorage.getItem('userID')

    // Populate the home screen with missions that the user can accept
    await axios
      .get('http://localhost:5201/mission/' + this.userID)
      .then((res) => {
        this.allMissions = res.data.data.missions
      })
      .catch(() => {
        alert('There is no more missions available at the moment')
        this.showInProgress = true
      })
  },
  methods: {
    async updateMissionsByStatus(status) {
      // Depending on the status, it will get the populate the homescreen with either the mission (available or inprogress)
      if (status == false) {
        await axios
          .get('http://localhost:5201/mission/' + this.userID)
          .then((res) => {
            this.allMissions = res.data.data.missions
          })
          .catch(() => {
            alert('There is no more missions available at the moment')
            this.showInProgress = true
          })
      } else {
        const response = await axios.get('http://localhost:5201/in_progress_mission/' + this.userID)
        console.log(response.data)
        this.allMissions = response.data.data.missions
      }
    },

    async handleSubmit(missionID) {
      // Handle the submission of the mission verification code to the backend based on the mission
      const inputVal = document.getElementById(missionID).value
      const params = {
        missionid: missionID,
        userid: this.userID,
        verification_code: inputVal
      }

      await axios.post('http://localhost:5201/complete_mission', params).catch(() => {
        alert('There seems to be an error that has occurred with the submission. Try again :)')
      })

      this.showInProgress = false
    },

    async acceptMission(missionID) {
      const params = {
        missionid: missionID,
        userid: this.userID
      }
      await axios
        .post('http://localhost:5201/accept_mission', params)
        .then(() => {
          // Updates the local state by loop thru and removing the mission id that is the same
          for (const iterator in this.allMissions) {
            if (this.allMissions[iterator].missionID == missionID) {
              this.allMissions.splice(iterator, 1)
            }
          }
        })
        .catch((err) => {
          console.error(err)
        })
    }
  }
}
</script>
