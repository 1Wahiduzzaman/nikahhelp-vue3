<template>
  <div>
    <Loader v-if="isLoading" :isLoading="isLoading" />
<!--    Short list candidate-->
<!--    team listed candidate-->
    <div v-else>
      <TeamOffRedirection v-if="redirection" />
      <div class="shortlist-content-wrapper">
        <div class="main-content-1">
          <div 
            v-if="getDisplayName === 'xs'" 
            class="w-100 text-center connection-filter-button"
            @click="showVTabs = !showVTabs">
            Connection Filter
          </div>
          <v-tabs 
            v-if="getDisplayName !== 'xs'" 
            color="indigo accent-4" 
            active-class="active-v-tab" 
            class="w-full d-flex justify-content-between support-tab" 
            :vertical="getDisplayName === 'xs'" 
            :grow="getDisplayName === 'xs'"
          >
            <v-tab href="#tab-1" @click="tab = 'tab-1'" class="font-weight-bold">All ({{ fullData.length }})</v-tab>
            <v-tab href="#tab-2" @click="tab = 'tab-2'" class="font-weight-bold">Shortlisted Candidate ({{ shortlistedData.length }})</v-tab>
            <v-tab href="#tab-3" @click="tab = 'tab-3'" class="font-weight-bold">Team listed Candidate ({{ teamlistedData.length }})</v-tab>
          </v-tabs>

          <transition name="shrink">
            <v-tabs 
              color="indigo accent-4" 
              active-class="active-v-tab" 
              class="w-full2 d-flex justify-content-between support-tab mt-2 mobile-v-tabs" 
              :vertical="getDisplayName === 'xs'" 
              :grow="getDisplayName === 'xs'"
              v-if="showVTabs"
              v-model="tab"
            >
              <v-tab href="#tab-1" @click="tab = 'tab-1'; showVTabs = !showVTabs" class="font-weight-bold">All ({{ fullData.length }})</v-tab>
              <v-tab href="#tab-2" @click="tab = 'tab-2'; showVTabs = !showVTabs" class="font-weight-bold">Shortlisted Candidate ({{ shortlistedData.length }})</v-tab>
              <v-tab href="#tab-3" @click="tab = 'tab-3'; showVTabs = !showVTabs" class="font-weight-bold">Team listed Candidate ({{ teamlistedData.length }})</v-tab>
            </v-tabs>
          </transition>

          <v-tabs-window v-model="tab">
            <v-tabs-window-item value="tab-1">
              <div class="row mt-2 mb-4 pl-sm-2">
                <div class="col-12 col-md-6 col-lg-3 px-sm-2" v-for="(shortlist, findex) in fullData" :key="findex">
                  <candidate-grid :item="shortlist"
                                  :shortListedIds="shortListedIds"
                                  :teamListedIds="teamListedIds"
                                  @loadList="loadList"
                                  @socketNotification="socketNotification" />
                </div>
              </div>
            </v-tabs-window-item>
            <v-tabs-window-item value="tab-2">
              <div class="row mt-2 mb-4">
                <div class="col-12 col-md-6 col-lg-3 px-2" v-for="(shortlist, sindex) in shortlistedData" :key="sindex">
                  <candidate-grid :item="shortlist"
                                  :shortListedIds="shortListedIds"
                                  :teamListedIds="teamListedIds"
                                  @loadList="loadList"
                                  @socketNotification="socketNotification" />
                </div>
              </div>
            </v-tabs-window-item>
            <v-tabs-window-item value="tab-3">
              <div class="row mt-2 mb-4">
                <div class="col-12 col-md-6 col-lg-3 px-2" v-for="(shortlist, tindex) in teamlistedData" :key="tindex">
                  <candidate-grid :item="shortlist"
                                  :shortListedIds="shortListedIds"
                                  :teamListedIds="teamListedIds"
                                  @loadList="loadList"
                                  @socketNotification="socketNotification" />
                </div>
              </div>
            </v-tabs-window-item>
          </v-tabs-window>

          <!-- Shortlisted Section Header -->
          <div class="d-none">
            <div class="item" style="text-align: left; width: 100%">
              <span
                class="item-number flex-center-center"
                style="float: left"
                >{{ shortListedCandidates.length }}</span
              >
              <span class="m-2 fw-600 fs-18">Shortlisted Candidate</span>
              <a
                href="/shortlist/all"
                type="submit"
                class="btn btn-sm btn-primary btn-round btn-submit float-right"
              >
                See All
              </a>
            </div>
            <div class="shortlisted-candidate">
              <!-- Shortlisted component goes here -->
              <candidate
                v-for="candidate in shortListedCandidates"
                :key="candidate.id"
                :candidate="candidate"
                @selected-candidate="selectedCandidate"
                @get-candidate-id="getCandidateId"
                @get-shortlist-id="removeShortlist"
                @connect-request="getCandidateTeamId"
                @block-candidate="blockCandidate"
              ></candidate>
              <select-team-for-teamlist
                :selectTeamModal="selectTeamModal"
                @handle-cancel="selectTeamModal = false"
                @handle-team="storeTeamlist"
              ></select-team-for-teamlist>
              <select-team-modal
                :selectTeamModal="selectTeamForConnect"
                @handle-cancel="selectTeamForConnect = false"
                @handle-team="connectRequest"
              ></select-team-modal>
            </div>
          </div>
          <!-- Team Listed -->
          <div class="mt-4 d-none">
            <!-- Team Listed Section Header -->
            <hr />
            <div class="item" style="text-align: left; width: 100%">
              <span
                class="item-number flex-center-center"
                style="float: left"
                >{{ teamShortListedCandidates.length }}</span
              >
              <span class="m-2 fw-600 fs-18">Team Listed Candidate</span>
              <a
                href="/shortlist/all/team"
                type="submit"
                class="btn btn-sm btn-primary btn-round btn-submit float-right"
              >
                See All
              </a>
            </div>
            <div class="teamlisted-candidate">
              <!-- Team listed component goes here	 -->
              <candidate
                v-for="candidate in teamShortListedCandidates"
                :key="candidate.id"
                :candidate="candidate"
                @selected-candidate="selectedCandidate"
                @get-candidate-id="getCandidateId"
                @get-shortlist-id="removeShortlist"
                @store-shortlist="storeShortlist"
                @connect-request="getCandidateTeamId"
                @block-candidate="blockCandidate"
              ></candidate>
              <select-team-modal
                :selectTeamModal="selectTeamForConnect"
                @handle-cancel="selectTeamForConnect = false"
                @handle-team="connectRequest"
              ></select-team-modal>
            </div>
          </div>
        </div>
        <div class="main-content-2 d-none">
          <div v-if="loadingSpinner">
            <loading-spinner></loading-spinner>
          </div>
          <div v-else>
            <div
              v-if="candidateProfileInfo"
              class="shadow-default profile-overview"
            >
              <h6>This Profile overview</h6>
              <hr />
              <div>
                <!-- Name -->
                <li class="flex-between-start">
                  <span class="flex-50 label-text">Name</span
                  ><span class="flex-50"
                    >:
                    <span class="ml-3"
                      >{{ candidateProfileInfo.first_name }}
                      {{ candidateProfileInfo.last_name }}</span
                    ></span
                  >
                </li>
                <!-- Age -->
                <li class="flex-between-start">
                  <span class="flex-50 label-text">Age</span
                  ><span class="flex-50"
                    >:
                    <span class="ml-3">{{
                      candidateProfileInfo.personal.dob
                    }}</span></span
                  >
                </li>
                <!-- height -->
                <li class="flex-between-start">
                  <span class="flex-50 label-text">Height</span
                  ><span class="flex-50"
                    >:
                    <span class="ml-3">{{
                      candidateProfileInfo.personal.per_height
                    }}</span></span
                  >
                </li>
                <!-- Nationality -->
                <li class="flex-between-start">
                  <span class="flex-50 label-text">Nationality</span
                  ><span class="flex-50"
                    >:
                    <span class="ml-3">{{
                      $store.state.candidateInfo.countries[
                        candidateProfileInfo.personal.per_nationality
                      ].name
                    }}</span></span
                  >
                </li>
                <!-- Ethnicity -->
                <li class="flex-between-start">
                  <span class="flex-50 label-text">Ethnicity</span
                  ><span class="flex-50 d-inherit"
                    >:
                    <span class="ml-3">
                      {{ candidateProfileInfo.personal.per_ethnicity }}</span
                    ></span
                  >
                </li>
                <!-- Country Of Birth -->
                <li class="flex-between-start">
                  <span class="flex-50 label-text">Country of Birth</span
                  ><span class="flex-50"
                    >:
                    <span class="ml-3">{{
                      $store.state.candidateInfo.countries[
                        candidateProfileInfo.personal.per_country_of_birth
                      ].name
                    }}</span></span
                  >
                </li>
                <!-- Current Residence -->
                <li class="flex-between-start">
                  <span class="flex-50 label-text">Current Residance</span
                  ><span class="flex-50"
                    >:
                    <span class="ml-3">{{
                      $store.state.candidateInfo.countries[
                        candidateProfileInfo.personal.per_current_residence
                      ].name
                    }}</span></span
                  >
                </li>
                <!-- Education -->
                <li
                  class="flex-between-start"
                  v-if="candidateProfileInfo.personal.per_education_level_id"
                >
                  <span class="flex-50 label-text">Education</span
                  ><span class="flex-50"
                    >:
                    <span class="ml-3">
                      <!-- {{
											$store.state.candidateInfo.study_level_options[
												candidateProfileInfo.personal.per_education_level_id
											].name
										}} -->
                      {{ personalStudyLevel }}
                    </span></span
                  >
                </li>
              </div>

              <h6 class="mt-3">This Profile Partner Preferences</h6>
              <hr />
              <div>
                <!-- Age -->
                <li class="flex-between-start">
                  <span class="flex-50 label-text">Age</span
                  ><span class="flex-50"
                    >:
                    <span class="ml-3">
                      {{ candidateProfileInfo.preference.pre_partner_age_min }}
                      to
                      {{ candidateProfileInfo.preference.pre_partner_age_max }}
                      years</span
                    ></span
                  >
                </li>
                <!-- Height -->
                <li class="flex-between-start">
                  <span class="flex-50 label-text">Height</span
                  ><span class="flex-50"
                    >:
                    <span class="ml-3"
                      >{{ candidateProfileInfo.preference.pre_height_min }} inch
                      to
                      {{ candidateProfileInfo.preference.pre_height_max }}
                      inch</span
                    ></span
                  >
                </li>
                <!-- Religion -->
                <li class="flex-between-start">
                  <span class="flex-50 label-text">Religion</span
                  ><span class="flex-50"
                    >:
                    <span class="ml-3"> Islam </span>
                  </span>
                </li>
                <!-- Nationality -->
                <li class="flex-between-start">
                  <span class="flex-50 label-text">Nationality</span
                  ><span class="flex-50 d-inherit"
                    >:
                    <span class="ml-3">
                      <div
                        v-for="nationality in candidateProfileInfo.preference
                          .preferred_nationality"
                        :key="nationality.id"
                      >
                        {{ nationality.name }}
                      </div>
                    </span></span
                  >
                </li>
                <!-- Ethnicity -->
                <li class="flex-between-start">
                  <span class="flex-50 label-text">Ethnicity</span
                  ><span class="flex-50 d-inherit"
                    >:
                    <span class="ml-3">
                      {{
                        candidateProfileInfo.preference.pre_ethnicities
                      }}</span
                    ></span
                  >
                </li>
                <!-- Country Of Birth -->
                <!-- Current Residence -->
                <!-- Education -->
                <li
                  class="flex-between-start"
                  v-if="candidateProfileInfo.preference.pre_study_level_id"
                >
                  <span class="flex-50 label-text">Education</span
                  ><span class="flex-50"
                    >:
                    <span class="ml-3">
                      {{ studyLevel }}
                    </span></span
                  >
                </li>
                <!-- Profession -->
                <!-- Occupation -->
                <li class="flex-between-start">
                  <span class="flex-50 label-text">Ocupation</span
                  ><span class="flex-50"
                    >:
                    <span class="ml-3">{{
                      candidateProfileInfo.preference.pre_occupation
                    }}</span></span
                  >
                </li>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Header from "@/components/dashboard/layout/Header.vue";
import Sidebar from "@/components/dashboard/layout/Sidebar.vue";
import LoadingSpinner from "@/components/ui/LoadingSpinner.vue";
import Footer from "@/components/dashboard/layout/Footer.vue";
import SelectTeamModal from "@/components/team/Modals/SelectTeamModal.vue";
import SelectTeamForTeamlist from "@/components/team/Modals/SelectTeamForTeamlist.vue";
import JwtService from "@/services/jwt.service";
import { openModalRoute } from "@/plugins/modal/modal.mixin";
import Candidate from "@/components/shortlist/Candidate.vue";
import CandidateGrid from "../../components/shortlist/CandidateGrid.vue";
import ApiService from '@/services/api.service';
import TeamOffRedirection from "../../components/redirection/TeamOffRedirection.vue";
import Notification from '@/common/notification.js';
export default {
  name: "Shortlist",
  components: {
    TeamOffRedirection,
    CandidateGrid,
    Header,
    Sidebar,
    Footer,
    LoadingSpinner,
    Candidate,
    SelectTeamModal,
    SelectTeamForTeamlist,
    // ShortlistedCandidate
  },
  sockets: {
    connect: function () {
      console.log("socket connected");
    },
    ping: function (data) {
      console.log(
          'this method was fired by the socket server. eg: io.emit("customEmit", data)'
      );
    },
  },
  data() {
    return {
      tab: 'tab-1',
      redirection: false,
      isLoading: false,
      loadingSpinner: false,
      showVTabs: false,
      user: {},
      is_verified: 1,
      error: null,
      candidateInfo: null,
      partnerInfo: null,
      userProfile: null,
      candidateProfileInfo: null,
      representativeProfileInfo: null,
      selectTeamModal: false,
      selectTeamForConnect: false,
      candidateId: null,
      candidateTeamId: null,
      fullData: [],
      shortlistedData: [],
      teamlistedData: [],
      shortListedIds: [],
      teamListedIds: []
    };
  },
  created() {
    this.getActiveTeamId();
  },
  computed: {
    shortListedCandidates() {
      const candidates = this.$store.getters["shortListedCandidates"];
      return candidates.filter((candidate) => {
        if (candidate.team_id == null) {
          return true;
        }
        return false;
      });
    },
    teamShortListedCandidates() {
      const candidates = this.$store.getters["teamShortListedCandidates"];
      return candidates;
    },
    personalStudyLevel() {
      if (this.candidateProfileInfo.personal.per_education_level_id) {
        var results =
          this.$store.state.candidateInfo.study_level_options.filter(
            (level) => {
              return (
                level.id ==
                this.candidateProfileInfo.personal.per_education_level_id
              );
            }
          );
        return results[0].name;
      } else {
        return null;
      }
    },
    studyLevel() {
      if (this.candidateProfileInfo.preference.pre_study_level_id) {
        var results =
          this.$store.state.candidateInfo.study_level_options.filter(
            (level) => {
              return (
                level.id ==
                this.candidateProfileInfo.preference.pre_study_level_id
              );
            }
          );
        return results[0].name;
      } else {
        return null;
      }
    },
    getDisplayName() {
      return this.$vuetify.display.name;
    },
    isWebSocketReady() {
      return this.$webSocket.readyState === 1;
    }
  },
  methods: {
    socketNotification(payload) {
      let loggedUser = JSON.parse(localStorage.getItem('user'));
      payload.sender = loggedUser.id;
      Notification.storeNotification(payload);
      payload.created_at = new Date();
      payload.seen = 0;
      payload.sender = loggedUser;
      if(payload && payload.receivers.length > 0) {
        payload.receivers = payload.receivers.map(item => {
          return item.toString();
        });

        if(this.isWebSocketReady) {
          this.$webSocket.send(JSON.stringify({
            action: 'notification',
            data: payload
          }));
        }
      }
      this.loadList();
    },
    loadList() {
      this.getActiveTeamId();
    },
    getActiveTeamId() {
      if (!JwtService.getTeamIDAppWide()) {
        // this.isLoading = true;
        // setTimeout(() => {
        //   this.isLoading = false;
        //   openModalRoute(this, "manage_team_redirect");
        // }, 2000);
        this.redirection = true;
      } else {
        this.fullData = [];
        this.shortlistedData = [];
        this.teamlistedData = [];
        this.loadShortListedCandidates();
        this.loadTeamShortListedCandidates();
        this.$store.dispatch("getCountries");
        this.$store.dispatch("getStudyLevelOptions");
        this.$store.dispatch("getReligionOptions");
        this.$store.dispatch("getOccupations");
      }
    },
    async loadShortListedCandidates() {
      this.isLoading = true;
      try {
        await ApiService.get(`/v1/short-listed-candidates`).then(res =>{
          this.isLoading = false;
          this.shortlistedData = res.data.data.map(item => {
            item.from_short_list = true;
            return item;
          });
          let data = [...this.fullData, ...this.shortlistedData];
          this.fullData = data;

          this.shortListedIds = res.data.data.map(item => {
            return parseInt(item.user_id);
          });
        }).catch(e => {
          console.log(e);
          this.isLoading = false;
        });;
        // this.shortlistedData = data;
        // let {data} = await this.$store.dispatch("loadShortListedCandidates");
      } catch (error) {
        this.error = error.message || "Something went wrong";
        console.log(this.error);
      }
      this.isLoading = false;
    },
    async loadTeamShortListedCandidates() {
      this.isLoading = true;
      try {
        await ApiService.get(`/v1/team-short-listed-candidates`).then(res =>{
          this.isLoading = false;
          this.teamlistedData = res.data.data.map(item => {
            item.from_team_list = true;
            item.is_team_listed = true;
            return item;
          });
          let data = [...this.fullData, ...this.teamlistedData];
          this.fullData = data;

          this.teamListedIds = res.data.data.map(item => {
            return parseInt(item.user_id);
          });
        }).catch(e => {
          console.log(e);
          this.isLoading = false;
        });
        // await this.$store.dispatch("loadTeamShortListedCandidates");
      } catch (error) {
        this.error = error.message || "Something went wrong";
        console.log(this.error);
      }
      this.isLoading = false;
    },
    async loadUserProfile(id) {
      this.loadingSpinner = true;

      try {
        const payload = {
          id,
        };
        const response = await this.$store.dispatch("getUserProfile", payload);
        //console.log(response);
        this.userProfile = response.data.user;
        //console.log(this.userProfile);
        if (response.data.user.account_type == 1) {
          this.candidateProfileInfo = response.data.candidate_information;
        } else {
          this.representativeProfileInfo =
            response.data.representative_information[0];
        }
        // this.user = this.$store.getters["userProfileInfo"];
        // this.candidateProfileInfo = this.$store.getters["candidateProfileInfo"];
        // this.representativeProfileInfo = this.$store.getters[
        // 	"representativeProfileInfo"
        // ];

        // if (data_input_status == 12) {
        // 	this.$router.push("/candidate-registration");
        // }
        // if (data_input_status == 22) {
        // 	this.$router.push("/representative-registration");
        // }
      } catch (error) {
        this.error = error.message || "Something went wrong";
        this.$error({
          title: "Error!",
          content: this.error,
        });
        //alert(this.error);
      }
      this.loadingSpinner = false;
    },
    async selectedCandidate(candidate) {
      await this.loadUserProfile(candidate.user_id);
      //this.candidateInfo = candidate.candidate;
      //this.partnerInfo = candidate.partner;
      //console.log(this.candidateInfo);
      console.log(this.candidateProfileInfo);
    },
    getCandidateId(candidateId) {
      //this.selectTeamModal = true;
      this.candidateId = candidateId;
      this.storeTeamlist();
    },
    storeShortlist(candidateId) {
      const vm = this;
      this.$confirm({
        title: "Do you want to shortlist this candidate?",
        content: "Are you sure?",
        okText: "Yes",
        okType: "primary",
        cancelText: "No",
        async onOk() {
          const payload = {
            user_id: candidateId,
            shortlisted_by: vm.user.id,
          };
          const response = vm.$store.dispatch("storeShortlist", payload); // Action in the shortlist module in action
          response.then((data) => {
            console.log(data);
            vm.$message.success("Candidate Shortlist Done");
            setTimeout(() => vm.$router.go(), 1000);
          });
        },
        onCancel() {
          console.log("Cancel");
        },
      });
      // const payload = {
      // 	user_id: candidateId,
      // 	shortlisted_by: this.user.id,
      // };
      // const response = this.$store.dispatch("storeShortlist", payload); // Action in the shortlist module in action
      // response.then((data) => {
      // 	console.log(data);
      // 	this.$message.success("Candidate Shortlist Done");
      // });
    },
    removeShortlist(shortlistId) {
      const vm = this;
      this.$confirm({
        title: "Are you sure?",
        content: "Do you want to remove this user from your shortlist?",
        okText: "Yes",
        okType: "danger",
        cancelText: "No",
        async onOk() {
          const response = vm.$store.dispatch("removeShortlist", shortlistId); // In Shorltist Module
          response
            .then((data) => {
              console.log(data);
              vm.$message.success(
                "Shortlisted candidate removed successfully!"
              );
              setTimeout(() => vm.$router.go(), 1000);
            })
            .catch((error) => {
              console.log(error);
            });
        },
        onCancel() {
          console.log("Cancel");
        },
      });
    },
    storeTeamlist() {
      let teamTableId = JwtService.getTeamID();
      const _payload = {
        user_id: this.candidateId,
        shortlisted_by: this.user.id,
        shortlisted_for: teamTableId,
      };
      console.log(_payload);
      const response = this.$store.dispatch("storeTeamlist", _payload); // Action in shortlist module
      response
        .then((data) => {
          console.log(data);
          this.selectTeamModal = false;
          this.$message.success("Team Listed candidate added successfully!");
          setTimeout(() => this.$router.go(), 1000);
        })
        .catch((error) => {
          console.log(error);
        });
    },

    getCandidateTeamId(candidateTeamId) {
      this.candidateTeamId = candidateTeamId;
      console.log(this.candidateTeamId, 'get candidate team id');
      // if (!this.$store.state.team.teamInfo) {
      // 	this.selectTeamForConnect = true;
      // } else {
      // 	console.log(this.$store.state.team.teamInfo);

      // 	this.connectRequest(this.$store.state.team.teamInfo);
      // }
      let teamId = JwtService.getTeamIDAppWide();
      this.connectRequest(teamId);
    },

    connectRequest(teamId) {
      const payload = {
        from_team_id: teamId,
        to_team_id: this.candidateTeamId,
      };
      console.log(payload);

      const response = this.$store.dispatch("connectWithTeam", payload);
      response
        .then((data) => {
          console.log(data);
          this.$success({
            title: "Connection Request Sent Successfully!",
            content: data.message,
            centered: true,
          });
        })
        .catch((error) => {
          console.log(error);
          this.$error({
            title: "Connection Request send not successful!",
            content: error.response.data.message,
            centered: true,
          });
        });
      this.selectTeamForConnect = false;
    },
    blockCandidate(candidateId) {
      const vm = this;
      this.$confirm({
        title: "Are you sure?",
        content: "Do you want to block this candidate?",
        okText: "Yes",
        okType: "danger",
        cancelText: "No",
        async onOk() {
          const payload = {
            user_id: candidateId,
            block_by: vm.user.id,
            type: "single",
          };
          await vm.$store.dispatch("blockCandidate", payload);
          vm.$message.success("Candidate block listed successfully");
          vm.$router.go();
        },
        onCancel() {
          console.log("Cancel");
        },
      });
    },
  },
};
</script>

<style scoped lang="scss">
@import "@/styles/base/_variables.scss";

.w-full2 {
  width: calc(100% - 30px) !important;
  margin: auto !important;
  margin-top: 8px !important;
}
.shortlist-content-wrapper {
  flex-wrap: wrap;
  margin-top: 4px;
  .main-content-1 {
    width: 100%;
    //margin: 4px 10px 20px;
    padding: 12px 15px 12px 15px;
    //margin-left: 260px;
    //margin-right: -800px;
    //@media (max-width: 1024px) {
    //  width: calc(100% - 270px);
    //}

    .connection-filter-button {
      border-radius: 5px; 
      background: #6159a7; 
      color: white; 
      height: 48px; 
      line-height: 48px; 
      font-weight: bold; 
      cursor: pointer;

      &:hover {
        background: #6159a780;
      }
    }

    .active-v-tab {
      background: #6159a7;
      color: #fff;
      border-radius: 5px;
    }

    .mobile-v-tabs {
      position: fixed;
      z-index: 1050;
    }
  
    .v-tabs-slider-wrapper {
      height: 3px !important;
  
      .v-tabs-slider {
        background-image: linear-gradient(#fff 50%, #6159a7 50%);
      }
    }
    .item {
      display: inline-block;
      button {
        img {
          width: 20px;
        }
      }
      .item-number {
        width: 40px;
        height: 40px;
        border-radius: 50%;
        background: $bg-secondary;
        color: $color-white;
        margin-left: 8px;
      }
    }
    .shortlisted-candidate {
      max-height: 350px;
      overflow-y: auto;
    }
    .teamlisted-candidate {
      max-height: 350px;
      overflow-y: auto;
    }
  }
  .main-content-2 {
    margin: 15px;
    width: 300px;
    height: 100%;
    .profile-overview {
      padding: 10px;
      height: 100%;
    }
  }
}
.shrink-enter-active {
  transition: all .3s ease;
}
.shrink-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.shrink-enter, .shrink-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translatey(-288px);
  opacity: 0;
}
.v-sheet.v-card:not(.v-sheet--outlined) {
  box-shadow: none !important;
  border: 2px solid #dddddd78;
}
</style>
