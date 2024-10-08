<template>
  <div>
    <div>
      <Loader v-if="isLoading" :isLoading="isLoading" />
      <div v-else>
        <TeamOffRedirection v-if="redirection" />
        <div v-else class="row" :class="{'loader-opacity pointer-events-none': innerLoading}">
          <div class="col-12">
            <div class="main-content-1">
<!--              <div-->
<!--                class="-->
<!--                  d-flex-->
<!--                  justify-content-between-->
<!--                  mb-3-->
<!--                  align-items-center-->
<!--                  mr-4-->
<!--                "-->
<!--              >-->
<!--                <h5 class="mt-2 px-4 connect-heading-text">-->
<!--                  Recent all connection activity <span class="mr-2"></span>-->
<!--                </h5>-->
<!--                <div class="d-flex align-items-center">-->
<!--                  &lt;!&ndash;                    <div class="cursor-pointer" @click="displayMode = 'grid'">&ndash;&gt;-->
<!--                  &lt;!&ndash;                      <img src="@/assets/icon/grid_icon.svg" alt="icon" width="30" class="opacity-60" :class="{'opacity-100': displayMode === 'grid'}" />&ndash;&gt;-->
<!--                  &lt;!&ndash;                    </div>&ndash;&gt;-->
<!--                  &lt;!&ndash;                    <div class="cursor-pointer ml-4" @click="displayMode = 'list'">&ndash;&gt;-->
<!--                  &lt;!&ndash;                      <img src="@/assets/icon/list_icon.svg" alt="icon" width="25" class="opacity-60" :class="{'opacity-100': displayMode === 'list'}" />&ndash;&gt;-->
<!--                  &lt;!&ndash;                    </div>&ndash;&gt;-->
<!--                  <select-->
<!--                    v-if="user.account_type === 3"-->
<!--                    v-model="teamId"-->
<!--                    class="custom-select w-50"-->
<!--                  >-->
<!--                    <option-->
<!--                      v-for="team in teams"-->
<!--                      :key="team.id"-->
<!--                      :value="team.team_id"-->
<!--                    >-->
<!--                      {{ team.name }}-->
<!--                    </option>-->
<!--                  </select>-->
<!--                </div>-->
<!--              </div>-->

              <div 
                v-if="getDisplayName === 'xs'" 
                class="w-100 text-center connection-filter-button"
                @click="showVTabs = !showVTabs">
                Connection Filter
              </div>

              <v-tabs 
                color="indigo accent-4" 
                active-class="active-v-tab" 
                class="w-full d-flex justify-content-between support-tab"
                :vertical="getDisplayName === 'xs'" 
                :grow="getDisplayName === 'xs'"
                v-if="getDisplayName !== 'xs'"
              >
                <v-tab href="#tab-1" @click="tab = 'tab-1', connection_type_choosed = 'all'" class="font-weight-bold">All ({{ connectionReports.result.length }})</v-tab>
                <v-tab href="#tab-2" @click="tab = 'tab-2', connection_type_choosed = 'connected'" class="font-weight-bold">Connected ({{connectionReports.connected_teams }}) </v-tab>
                <v-tab href="#tab-3" @click="tab = 'tab-3', connection_type_choosed = 'Request received'" class="font-weight-bold">Received ({{ connectionReports.request_received }})</v-tab>
                <v-tab href="#tab-4" @click="tab = 'tab-4', connection_type_choosed = 'Request sent'" class="font-weight-bold">Sent ({{ connectionReports.request_sent }})</v-tab>
                <v-tab href="#tab-5" @click="tab = 'tab-5', connection_type_choosed = 'we declined'" class="font-weight-bold">We declined ({{ connectionReports.we_declined}})</v-tab>
                <v-tab href="#tab-6" @click="tab = 'tab-6', connection_type_choosed = 'they declined'" class="font-weight-bold">They declined ({{ connectionReports.they_declined }})</v-tab>
              </v-tabs>
              <transition name="shrink">
                <v-tabs 
                  color="indigo accent-4" 
                  active-class="active-v-tab" 
                  class="w-full2 d-flex justify-content-between support-tab ml-sm-3 mt-2 mobile-v-tabs"
                  :vertical="getDisplayName === 'xs'" 
                  :grow="getDisplayName === 'xs'"
                  v-model="tab"
                  v-if="showVTabs"
                >
                  <v-tab 
                    href="#tab-1" 
                    @click="tab = 'tab-1', connection_type_choosed = 'all'; showVTabs = !showVTabs" 
                    class="font-weight-bold"
                  >
                    All ({{ connectionReports.result.length }})
                  </v-tab>
                  <v-tab 
                    href="#tab-2" 
                    @click="tab = 'tab-2', connection_type_choosed = 'connected'; showVTabs = !showVTabs" 
                    class="font-weight-bold"
                  >
                    Connected ({{connectionReports.connected_teams }}) 
                  </v-tab>
                  <v-tab 
                    href="#tab-3" 
                    @click="tab = 'tab-3', connection_type_choosed = 'Request received'; showVTabs = !showVTabs" 
                    class="font-weight-bold"
                  >
                    Received ({{ connectionReports.request_received }})
                  </v-tab>
                  <v-tab 
                    href="#tab-4" @click="tab = 'tab-4', connection_type_choosed = 'Request sent'; showVTabs = !showVTabs" 
                    class="font-weight-bold"
                  >
                    Sent ({{ connectionReports.request_sent }})
                  </v-tab>
                  <v-tab 
                    href="#tab-5" @click="tab = 'tab-5', connection_type_choosed = 'we declined'; showVTabs = !showVTabs" 
                    class="font-weight-bold"
                  >
                    We declined ({{ connectionReports.we_declined}})
                  </v-tab>
                  <v-tab 
                    href="#tab-6" @click="tab = 'tab-6', connection_type_choosed = 'they declined'; showVTabs = !showVTabs" 
                    class="font-weight-bold"
                  >
                    They declined ({{ connectionReports.they_declined }})
                  </v-tab>
                </v-tabs>
              </transition>

              <v-tabs-window v-model="tab" class="tab-items mt-4">
                <v-tabs-window-item value="tab-1">
                  <div class="row px-3 tab-margin-top">
                    <div
                        class="col-12 col-md-6 col-xl-3"
                        v-for="(connection, connecIndex) in getFilteredConnections"
                        :key="connecIndex"
                    >
                      <candidate-grid-view
                          v-if="
                        displayMode === 'grid' &&
                        connectionReports.result &&
                        connectionReports.result.length > 0
                      "
                          :connection="connection"
                          :active_team_id="active_team_id"
                          @selected-connection="selectedConnection"
                          @accept-request="acceptRequest"
                          @disconnect-team="disconnectTeam"
                          @decline-request="declineRequest"
                          @connect-request="connectRequest"
                          @block-candidate="blockCandidate"
                          @decline-connection="declineConnection"
                      />
                    </div>
                  </div>
                </v-tabs-window-item>

                <v-tabs-window-item value="tab-2">
                  <div class="row px-3 tab-margin-top">
                    <div
                        class="col-12 col-md-6 col-xl-3"
                        v-for="(connection, connecIndex) in getFilteredConnections"
                        :key="connecIndex"
                    >
                      <candidate-grid-view
                          v-if="
                        displayMode === 'grid' &&
                        connectionReports.result &&
                        connectionReports.result.length > 0
                      "
                          :connection="connection"
                          @selected-connection="selectedConnection"
                          @accept-request="acceptRequest"
                          @disconnect-team="disconnectTeam"
                          @decline-request="declineRequest"
                          @connect-request="connectRequest"
                          @block-candidate="blockCandidate"
                          @decline-connection="declineConnection"
                      />
                    </div>
                  </div>
                </v-tabs-window-item>

                <v-tabs-window-item value="tab-3">
                  <div class="row px-3 tab-margin-top">
                    <div
                        class="col-12 col-md-6 col-xl-3"
                        v-for="(connection, connecIndex) in getFilteredConnections"
                        :key="connecIndex"
                    >
                      <candidate-grid-view
                          v-if="
                        displayMode === 'grid' &&
                        connectionReports.result &&
                        connectionReports.result.length > 0
                      "
                          :connection="connection"
                          @selected-connection="selectedConnection"
                          @accept-request="acceptRequest"
                          @disconnect-team="disconnectTeam"
                          @decline-request="declineRequest"
                          @connect-request="connectRequest"
                          @block-candidate="blockCandidate"
                          @decline-connection="declineConnection"
                      />
                    </div>
                  </div>
                </v-tabs-window-item>

                <v-tabs-window-item value="tab-4">
                  <div class="row px-3 tab-margin-top">
                    <div
                        class="col-12 col-md-6 col-xl-3"
                        v-for="(connection, connecIndex) in getFilteredConnections"
                        :key="connecIndex"
                    >
                      <candidate-grid-view
                          v-if="
                        displayMode === 'grid' &&
                        connectionReports.result &&
                        connectionReports.result.length > 0
                      "
                          :connection="connection"
                          @selected-connection="selectedConnection"
                          @accept-request="acceptRequest"
                          @disconnect-team="disconnectTeam"
                          @decline-request="declineRequest"
                          @connect-request="connectRequest"
                          @block-candidate="blockCandidate"
                          @decline-connection="declineConnection"
                      />
                    </div>
                  </div>
                </v-tabs-window-item>

                <v-tabs-window-item value="tab-5">
                  <div class="row px-3 tab-margin-top">
                    <div
                        class="col-12 col-md-6 col-xl-3"
                        v-for="(connection, connecIndex) in getFilteredConnections"
                        :key="connecIndex"
                    >
                      <candidate-grid-view
                          v-if="
                        displayMode === 'grid' &&
                        connectionReports.result &&
                        connectionReports.result.length > 0
                      "
                          :connection="connection"
                          @selected-connection="selectedConnection"
                          @accept-request="acceptRequest"
                          @disconnect-team="disconnectTeam"
                          @decline-request="declineRequest"
                          @connect-request="connectRequest"
                          @block-candidate="blockCandidate"
                          @decline-connection="declineConnection"
                      />
                    </div>
                  </div>
                </v-tabs-window-item>

                <v-tabs-window-item value="tab-6">
                  <div class="row px-3 tab-margin-top">
                    <div
                        class="col-12 col-md-6 col-xl-3"
                        v-for="(connection, connecIndex) in getFilteredConnections"
                        :key="connecIndex"
                    >
                      <candidate-grid-view
                          v-if="
                        displayMode === 'grid' &&
                        connectionReports.result &&
                        connectionReports.result.length > 0
                      "
                          :connection="connection"
                          @selected-connection="selectedConnection"
                          @accept-request="acceptRequest"
                          @disconnect-team="disconnectTeam"
                          @decline-request="declineRequest"
                          @connect-request="connectRequest"
                          @block-candidate="blockCandidate"
                          @decline-connection="declineConnection"
                      />
                    </div>
                  </div>
                </v-tabs-window-item>
              </v-tabs-window>

              <div
                v-if="connectionReports"
                class="d-none w-full flex-wrap ml-2"
              >
                <v-chip
                  class="ma-2 connected"
                  color="green"
                  text-color="white"
                  @click="connection_type_choosed = 'connected'"
                >
                  <v-avatar left class="green darken-4">
                    {{ connectionReports.connected_teams }}
                  </v-avatar>
                  Connected
                </v-chip>
                <v-chip
                  class="ma-2 orange"
                  text-color="white"
                  @click="connection_type_choosed = 'Request received'"
                >
                  <v-avatar left class="orange darken-4">
                    {{ connectionReports.request_received }}
                  </v-avatar>
                  Received
                </v-chip>
                <v-chip
                  class="ma-2"
                  color="cyan"
                  text-color="white"
                  @click="connection_type_choosed = 'Request send'"
                >
                  <v-avatar left class="cyan darken-4">
                    {{ connectionReports.request_sent }}
                  </v-avatar>
                  Sent
                </v-chip>
                <v-chip
                  class="ma-2"
                  color="pink"
                  text-color="white"
                  @click="connection_type_choosed = 'We declined'"
                >
                  <v-avatar left class="pink darken-4">
                    {{ connectionReports.we_declined }}
                  </v-avatar>
                  We declined
                </v-chip>
                <v-chip
                  class="ma-2"
                  color="indigo"
                  text-color="white"
                  @click="connection_type_choosed = 'They declined'"
                >
                  <v-avatar left class="indigo darken-4">
                    {{ connectionReports.they_declined }}
                  </v-avatar>
                  They declined
                </v-chip>
              </div>

<!--              <div v-if="connectionReports" class="shortlist-wrapper">-->
<!--                <div class="row px-3 tab-margin-top">-->
<!--                  <div-->
<!--                    class="col-12 col-md-4 col-xl-3 px-2 py-0"-->
<!--                    v-for="(connection, connecIndex) in getFilteredConnections"-->
<!--                    :key="connecIndex"-->
<!--                  >-->
<!--                    <candidate-grid-view-->
<!--                      v-if="-->
<!--                        displayMode === 'grid' &&-->
<!--                        connectionReports.result &&-->
<!--                        connectionReports.result.length > 0-->
<!--                      "-->
<!--                      :connection="connection"-->
<!--                      @selected-connection="selectedConnection"-->
<!--                      @accept-request="acceptRequest"-->
<!--                      @disconnect-team="disconnectTeam"-->
<!--                      @decline-request="declineRequest"-->
<!--                      @connect-request="connectRequest"-->
<!--                      @block-candidate="blockCandidate"-->
<!--                      @decline-connection="declineConnection"-->
<!--                    />-->
<!--                  </div>-->
<!--                </div>-->
<!--              </div>-->


              <template
                v-if="
                  displayMode === 'list' &&
                  connectionReports &&
                  connectionReports.result &&
                  connectionReports.result.length > 0
                "
              >
                <candidate
                  v-for="connection in connectionReports.result"
                  :key="connection.connection_id"
                  :connection="connection"
                  @selected-connection="selectedConnection"
                  @accept-request="acceptRequest"
                  @disconnect-team="disconnectTeam"
                  @decline-request="declineRequest"
                  @connect-request="connectRequest"
                  @block-candidate="blockCandidate"
                ></candidate>
              </template>
            </div>
          </div>
          <div class="col-12 col-xl-3 d-none">
            <!-- Connection Status -->
            <div class="main-content-2">
              <div class="connection-status p-3">
                <p class="fs-18 text-center">Connection Status</p>
                <hr />
                <!-- Connection stats -->
                <div v-if="connectionReports && !connectionOverview">
                  <!-- Teams Connected -->
                  <div class="row mt-3">
                    <div class="col-3">
                      <div class="item connected">
                        <span class="item-number flex-center-center fs-22">{{
                          connectionReports.connected_teams
                        }}</span>
                      </div>
                    </div>
                    <div class="col-9 mt-2 fs-18">Teams connected</div>
                  </div>
                  <!-- Teams Request Received -->
                  <div class="row mt-3">
                    <div class="col-3">
                      <div class="item received">
                        <span class="item-number flex-center-center fs-22">{{
                          connectionReports.request_received
                        }}</span>
                      </div>
                    </div>
                    <div class="col-9 fs-18">Teams Request Received</div>
                  </div>
                  <!-- Teams Request Sent -->
                  <div class="row mt-3">
                    <div class="col-3">
                      <div class="item request-sent">
                        <span class="item-number flex-center-center fs-22">{{
                          connectionReports.request_sent
                        }}</span>
                      </div>
                    </div>
                    <div class="col-9 mt-2 fs-18">Teams Request Sent</div>
                  </div>
                  <!-- We declined team request -->
                  <div class="row mt-3">
                    <div class="col-3">
                      <div class="item we-declined">
                        <span class="item-number flex-center-center fs-22">{{
                          connectionReports.we_declined
                        }}</span>
                      </div>
                    </div>
                    <div class="col-9 fs-18">We declined Team request</div>
                  </div>
                  <!-- They declined Our team request -->
                  <div class="row mt-3">
                    <div class="col-3">
                      <div class="item they-declined">
                        <span class="item-number flex-center-center fs-22">{{
                          connectionReports.they_declined
                        }}</span>
                      </div>
                    </div>
                    <div class="col-9 fs-18">
                      They declined Our team request
                    </div>
                  </div>
                </div>
                <div v-else>
                  <div class="pt-5">
                    <h6 class="pb-2">This Profile Connection Overview</h6>
                    <hr />

                    <div v-if="connectionOverview" class="connection-overview">
                      <table class="table table-borderless overview-table">
                        <tr>
                          <td class="td-60">Connection Status</td>
                          <td class="text-end">:</td>
                          <td>{{ connectionOverview.connection }}</td>
                        </tr>
                        <tr>
                          <td class="td-60">Connected date</td>
                          <td class="text-end">:</td>
                          <td>
                            {{
                              dateFromDateTime(connectionOverview.responded_at)
                            }}
                          </td>
                        </tr>
                        <tr>
                          <td class="td-60">Connection requested by</td>
                          <td class="text-end">:</td>
                          <td>
                            {{ connectionOverview.requested_by.full_name }}
                          </td>
                        </tr>
                        <tr>
                          <td class="td-60">Request Date</td>
                          <td class="text-end">:</td>
                          <td>
                            {{
                              dateFromDateTime(connectionOverview.requested_at)
                            }}
                          </td>
                        </tr>
                      </table>
                    </div>
                  </div>
                  <div class="pt-5 mt-5">
                    <h6 class="pb-2">This Profile Team Overview</h6>
                    <hr />
                    <div
                      v-if="connectionOverview"
                      class="profile-team-overview"
                    >
                      <table class="table table-borderless overview-table">
                        <tr>
                          <td class="td-60">Team name</td>
                          <td class="text-end">:</td>
                          <td>{{ connectionOverview.team_name }}</td>
                        </tr>
                        <tr>
                          <td class="td-60">Team members</td>
                          <td class="text-end">:</td>
                          <td>{{ connectionOverview.total_teamMember }}</td>
                        </tr>
                        <tr>
                          <td class="td-60">Team creation date</td>
                          <td class="text-end">:</td>
                          <td>
                            {{
                              dateFromTimeStamp(
                                connectionOverview.team_created_date
                              )
                            }}
                          </td>
                        </tr>
                        <tr>
                          <td class="td-60">Team created by</td>
                          <td class="text-end">:</td>
                          <td>{{ connectionOverview.team_created_by }}</td>
                        </tr>
                      </table>
                    </div>
                  </div>
                  <!-- {{ connectionOverview }} -->
                  <hr class="pb-2" />
                  <div class="flex justify-content-center">
                    <button
                      class="mt-2 btn btn-primary outline-border px-5"
                      @click="profileOverview()"
                    >
                      This Profile overview
                    </button>
                  </div>
                  <div class="flex justify-content-center mt-2">
                    <button class="mt-2 btn btn-primary outline-border px-5">
                      Profile completion status
                    </button>
                  </div>
                  <!--                    <div class="flex justify-content-center">-->
                  <!--                      <button-->
                  <!--                          class="mt-2 btn btn-primary outline-border"-->
                  <!--                          @click="gotoConnectionStatus"-->
                  <!--                      >-->
                  <!--                        Go Back to Connection Status-->
                  <!--                      </button>-->
                  <!--                    </div>-->
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="overlay-container" v-if="innerLoading">
          <v-overlay :opacity="1" :value="innerLoading">
            <v-progress-circular :size="100" :width="7" color="purple" indeterminate>
              Loading...
            </v-progress-circular>
          </v-overlay>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Candidate from "@/components/connections/Candidate.vue";
import JwtService from "@/services/jwt.service";
import { dateFromDateTime, dateFromTimeStamp } from "@/common/helpers.js";
import CandidateGridView from "../../components/connections/CandidateGridView.vue";
import Notification from "@/common/notification.js";
// import { openModalRoute } from "@/plugins/modal/modal.mixin";
import TeamOffRedirection from "../../components/redirection/TeamOffRedirection.vue";
// import ApiService from "../../services/api.service";
export default {
  name: "Connections",
  components: {
    TeamOffRedirection,
    CandidateGridView,
    Candidate,
  },
  data() {
    return {
      tab: 'tab-1',
      redirection: false,
      isLoading: false,
      user: {},
      is_verified: 1,
      error: null,
      connections: {},
      connectionReports: null,
      teams: null,
      teamId: null,
      connectionOverview: null,
      displayMode: "grid",
      connection_type_choosed: "all",
      active_team_id: null,
      innerLoading: false,
      showVTabs: false,
    };
  },
  computed: {
    getFilteredConnections() {
      if (
        this.connectionReports &&
        this.connectionReports.result &&
        this.connectionReports.result.length > 0
      ) {
        if (this.connection_type_choosed == "all") {
          return this.connectionReports.result;
        } else {
          return this.connectionReports.result.filter(
            (item) => item.connection_type == this.connection_type_choosed
          );
        }
      }
      return [];
    },
    getConnections() {
      if (
          this.connectionReports &&
          this.connectionReports.result &&
          this.connectionReports.result.length > 0
      ) {
        return this.connectionReports.result;
      }
      return [];
    },
    isWebSocketReady() {
      return this.$webSocket.readyState === 1;
    },
    // connectionStatus() {
    // 	return this.connectionOverview.connection_overview.connection_status;
    // },
    // connectedDate() {
    // 	if (this.connectionOverview.connection_overview.responded_at) {
    // 		const date =
    // 			this.connectionOverview.connection_overview.responded_at.split(" ");
    // 		return date[0];
    // 	} else return "N/A";
    // },
    // connectionRequestedBy() {
    // 	return this.connectionOverview.connection_overview.requested_by.full_name;
    // },
    // requestedDate() {
    // 	if (this.connectionOverview.connection_overview.requested_at) {
    // 		const date =
    // 			this.connectionOverview.connection_overview.requested_at.split(" ");
    // 		return date[0];
    // 	} else return "N/A";
    // },
    // teamName() {
    // 	return this.connectionOverview.profile_team_overview.team_name;
    // },
    // memberCount() {
    // 	return this.connectionOverview.profile_team_overview.member_count;
    // },
    // teamCreationDate() {
    // 	const date =
    // 		this.connectionOverview.profile_team_overview.team_creation_date.split(
    // 			"T"
    // 		);
    // 	return date[0];
    // },
    // teamCreatedBy() {
    // 	return this.connectionOverview.profile_team_overview.team_created_by
    // 		.full_name;
    // },
    getDisplayName() {
      return this.$vuetify.display.name;
    }
  },
  created() {
    this.getActiveTeamId();
  },
  watch: {
    teamId: function () {
      this.loadConnections();
    },
  },
  methods: {
    dateFromDateTime, //From helpers.js
    dateFromTimeStamp, //From helpers.js
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
        this.active_team_id = JwtService.getTeamIDAppWide();
        this.loadConnectionReports();
      }
    },
    loadConnections() {
      this.isLoading = true;
      try {
        //const teamId = "94e5e52b-c47f-4cc3-83fa-1e4b034e5650";
        const response1 = this.$store.dispatch("getTeams");
        response1.then((data) => {
          this.teams = data.data.data;
          if (JwtService.getTeamIDAppWide()) {
            this.teamId = JwtService.getTeamIDAppWide();
            console.log(this.teamId);
          } else if (localStorage.teamId && !this.teamId) {
            this.teamId = localStorage.teamId;
          } else {
            if (!this.teamId) {
              this.teamId = data.data.data[0].team_id;
            }
          }
          console.log(this.teamId);
          const teamId = this.teamId;
          console.log(teamId);
          const payload = {
            team_id: teamId,
          };
          const response = this.$store.dispatch("loadConnections", payload);
          response.then((data) => {
            this.connections = data.data;
            console.log(data.data);
          });
        });
      } catch (error) {
        this.error = error.message || "Something went wrong";
        console.log(this.error);
      }
      this.isLoading = false;
    },
    loadConnectionReports() {
      const teamId = JwtService.getTeamIDAppWide();
      this.isLoading = true;
      try {
        const response = this.$store.dispatch("loadConnectionReports", teamId);
        response
          .then((data) => {
            this.connectionReports = data.data.data;
            console.log(this.connectionReports, 'this.connection repors')
            this.isLoading = false;
          })
          .catch((error) => {
            console.log(error.response.data.message);
            this.isLoading = false;
          });
      } catch (error) {
        console.log(error.message);
      }
    },
    selectedConnection(data) {
      //this.connectionDetails = data;
      this.isLoading = true;
      console.log(data);
      this.connectionOverview = data;
      // try {
      // 	const teamId = data.team_id;
      // 	const connectionId = data.connection_id;
      // 	const payload = {
      // 		team_id: teamId,
      // 		connection_id: connectionId,
      // 	};
      // 	const response = this.$store.dispatch("getConnectionOverview", payload);
      // 	response.then((data) => {
      // 		this.connectionOverview = data.data;
      // 		console.log(data.data);
      // 	});
      // } catch (error) {
      // 	this.error = error.message || "Something went wrong";
      // 	console.log(this.error);
      // }
      this.isLoading = false;
    },
    gotoConnectionStatus() {
      this.connectionOverview = null;
    },
    changeTeam(data) {
      console.log(data);
      this.teamId = data;
    },
    acceptRequest(connectionId, notifyObj) {
      const payload = {
        request_id: connectionId,
        connection_status: "1",
      };
      //return;
      this.innerLoading = true;
      const response = this.$store.dispatch("respondToRequest", payload);
      response
        .then((data) => {
          this.innerLoading = false;
          this.socketNotification(notifyObj);

          const vm = this;
          this.$success({
            title: "Success",
            content: data.message,
            onOk() {
              setTimeout(() => {
                vm.$router.go();
              }, 500);
            },
          });
          // this.$message.success("Request Accepeted successfully!");

          // setTimeout(() => {
          // 	this.$router.go();
          // }, 1500);
        })
        .catch((error) => {
          console.log(error);
          this.$error({
            title: 'You do not have permission',
            content: 'You are not an admin. ' + error.response.data.message,
            center: true,
          })
          this.innerLoading = false;
        });
    },
    disconnectTeam(connection, notifyObj) {
      const payload = {
        connection_id: connection.connection_id,
      };
      console.log(payload);
      //return;
      this.innerLoading = true;
      const response = this.$store.dispatch("disconnectTeam", payload);
      response
        .then((data) => {
          console.log(data);
          this.innerLoading = false;
          const vm = this;

          vm.socketNotification(notifyObj);

          this.$success({
            title: "Success",
            content: data.message,
            onOk() {
              setTimeout(() => {
                vm.$router.go();
              }, 1500);
            },
          });
          //this.$message.success("Team Disconnected successfully!");
        })
        .catch((error) => {
          console.log(error.response.data.message);
          this.$error({
            title: 'You do not have permission',
            content: 'You are not an admin. ' + error.response.data.message,
            center: true,
          })
          this.innerLoading = false;
        });
    },
    declineRequest(data, notifyObj) {
      this.innerLoading = true;
      const response = this.$store.dispatch("respondToRequest", data);
      response
        .then((data) => {
          this.innerLoading = false;
          this.socketNotification(notifyObj);
          const vm = this;
          this.$success({
            title: "Success",
            content: data.message,
            onOk() {
              setTimeout(() => {
                vm.$router.go();
              }, 500);
            },
          });
          // this.$message.success("Request declined successfully!");

          // setTimeout(() => {
          // 	this.$router.go();
          // }, 1500);
        })
        .catch((error) => {
          console.log(error);
          this.innerLoading = false;
          this.$error({
            title: 'You do not have persmission',
            content: 'You are not an admin. ' + error.response.data.message,
            center: true,
          });

        });
    },
    connectRequest(to_team_id, notifyObj) {
      let teamId = JwtService.getTeamIDAppWide();
      const payload = {
        to_team_id: to_team_id,
        from_team_id: teamId,
      };
      console.log(payload);
      this.innerLoading = true;
      const response = this.$store.dispatch("connectWithTeam", payload);
      response
        .then((data) => {
          this.innerLoading = false;
          this.socketNotification(notifyObj);
          const vm = this;
          this.$success({
            title: "Success",
            content: data.message,
            onOk() {
              setTimeout(() => {
                vm.$router.go();
              }, 500);
            },
          });
          // this.$message.success("Connection Request Sent successfully!");

          // setTimeout(() => {
          // 	this.$router.go();
          // }, 1500);
        })
        .catch((error) => {
          console.log(error);
          this.innerLoading = false;
        });
    },
    blockCandidate(candidateId, connection, notifyObj) {
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
          vm.innerLoading = true;
          try {
             await vm.$store.dispatch("disconnectTeam", {
              connection_id: connection.connection_id,
            });
            vm.innerLoading = false;
  
            vm.socketNotification(notifyObj);
  
            vm.$success({
              title: "Success",
              content: "Candidate block listed successfully",
              onOk() {
                vm.$router.go();
              },
            });
          } catch(error) {
            vm.$error({
              title: 'You do not have permission',
              content: 'You are not an admin. ' + error.response.data.message,
              center: true,
            });
            vm.innerLoading = false;
            return;
          }

          try {
            await vm.$store.dispatch("blockCandidate", payload);
          } catch(error) {
            vm.$error({
              title: 'You do not have permission',
              content: 'You are not an admin. ' + error.response.data.message,
              center: true,

            });
            vm.innerLoading = false;
          }
          //vm.$message.success("Candidate block listed successfully");
          //vm.$router.go();
        },
        onCancel() {
          console.log("Cancel");
          vm.innerLoading = false;
        },
      });
    },
    declineConnection(connectionId) {
      const payload = {
        request_id: connectionId,
        connection_status: "2",
      };
      this.innerLoading = true;
      const response = this.$store.dispatch("respondToRequest", payload);
      response
          .then((data) => {
            console.log(data);
            this.innerLoading = false;
            const vm = this;
            this.$success({
              title: "Success",
              content: data.message,
              onOk() {
                setTimeout(() => {
                  vm.$router.go();
                }, 500);
              },
            });
            // this.$message.success("Request declined successfully!");

            // setTimeout(() => {
            // 	this.$router.go();
            // }, 1500);
          })
          .catch((error) => {
            console.log(error);
            this.innerLoading = false;
          });
    },
    profileOverview() {
      if (
        this.connectionOverview &&
        this.connectionOverview.candidateInfo &&
        this.connectionOverview.candidateInfo.candidate_userid
      ) {
        this.$router.push({
          name: "profile",
          query: {
            user_id: this.connectionOverview.candidateInfo.candidate_userid,
          },
        });
      }
    },
    prepareSocketData(data) {
      console.log(data);
      // let payload = {
      //   receivers: [],
      //   title: `disconnected this team`,
      //   team_temp_name: 'Team name',
      //   team_id: 2
      // };
      // this.socketNotification(payload);
    },
  },
};
</script>

<style scoped lang="scss">
@import "@/styles/base/_variables.scss";
.main-content-1 {
  width: 100%;
  //margin: 4px 0 20px;
  padding: 12px 15px 12px 15px;
  //margin-left: 260px;
  //@media (max-width: 1024px) {
  //  width: calc(100% - 270px);
  //}

  .tab-items {
    .col-12, .col-md-4, .col-xl-3 {
      margin: 0px !important;
      padding: 5px !important;
    }
  }
  .v-window {
    overflow: inherit !important;
  }
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
}
.main-content-2 {
  margin: 15px;
  //width: 300px;
  .connection-history {
    padding: 10px;
  }
  .connection-status {
    padding: 10px;
    .row {
      padding: 10px;
      .item {
        width: 50px;
        height: 50px;
        border-radius: 8px;
        display: flex;
        justify-content: center;
        align-items: center;
        margin: auto;
        transform: rotate(45deg);
        .item-text {
          font-size: 22px;
          font-weight: 700;
        }
        .item-number {
          color: white;
          font-size: 22px;
          transform: rotate(-45deg);
        }
      }

      .connected {
        background-color: $bg-success !important;
      }
      .received {
        background-color: $bg-warning;
      }
      .request-sent {
        background-color: $bg-info;
      }
      .we-declined {
        background-color: $bg-danger;
      }
      .they-declined {
        background-color: $bg-secondary;
      }
    }
    .connection-overview {
      p {
        font-size: 14px;
        font-weight: 600;
        color: #656565;
      }
    }
    .profile-team-overview {
      p {
        font-size: 14px;
        font-weight: 600;
        color: #656565;
      }
    }
  }
}
.opacity-60 {
  opacity: 0.6;
}
.w-full2 {
  width: calc(100% - 30px) !important;
  margin: auto !important;
  margin-top: 8px !important;
}
.opacity-100 {
  opacity: 1;
}
.footer {
  height: 20px;
}
.td-60 {
  width: 60%;
}
.tab-margin-top {
  margin-top: 1px;
}
.overview-table td,
.overview-table th {
  padding: 0.25rem 0.75rem;
}
.outline-border {
  border: 1px solid $color-white;
  outline-style: solid;
  outline-color: $bg-primary;
}
.connected {
  background-color: #3ab549 !important;
}
.received {
  background-color: #fbb03b !important;
}
.sent {
  background-color: #1bb9c2 !important;
}
.we-declined {
  background-color: #fa4942 !important;
}
.they-declined {
  background-color: #522e8e !important;
}
// .mobile-margin {
//   margin-left: -10px;
//   padding: 0 8px;
//   @media (min-width: 768px) {
//     margin-left: 0;
//   }
// }
.connect-heading-text {
  font-size: 14px;
  @media (min-width: 768px) {
    font-size: 18px;
  }
}
.loader-opacity {
  opacity: 0.4;
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
</style>
