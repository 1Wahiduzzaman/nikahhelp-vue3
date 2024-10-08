<template>
  <div>
    <div>
      <Loader v-if="isLoading" :isLoading="isLoading" />
      <div v-else>
        <div class="main-content-wrapper">
          <div class="main-content-1">
            <div
              class="
                flex
                border-bottom
                pb-4
                justify-content-between
                align-items-center
              "
            >
              <h4 class="d-sm-none d-md-block text--disabled">All Notifications</h4>
              <div
                class="
                  flex
                  justify-content-end
                  align-items-center
                  w-full
                  flex-wrap
                "
              >
                <span class="all-button px-4" style="height: 35px; border-radius: 20px" @click="notiType = 'all'">All</span>
                <span class="unread-button px-4 ml-2" style="height: 35px; border-radius: 20px" @click="notiType = 0">Unread</span>

<!--                <v-btn rounded color="success" dark class="ml-2 read-btn" small @click="markAllAsRead()">-->
<!--                  <a-icon type="check" color="success" class="pr-2" />-->
<!--                  Mark all as read-->
<!--                </v-btn>-->
              </div>
            </div>
            <div class="notification-page-height pr-3 mt-4">
              <notification
                v-for="(notification, index) in filteredNotifications"
                :key="index"
                :notification="notification"
              />
            </div>
          </div>
          <!-- <div class="main-content-2">
						<div class="shadow-default notifications-sidebar">
							<div class="filter-notification">
								<h6>Filter Notification By</h6>
								<button class="btn btn-primary btn-block mt-5 mb-5">
									Show Unseen Only
								</button>
								<button class="btn btn-primary btn-block mt-5 mb-5">
									Show Seen Only
								</button>
							</div>
							<div class="manage-notification">
								<h6>Manage Noification</h6>
								<button class="btn btn-primary btn-block">
									Mark all as Seen
								</button>
								<button class="btn btn-primary btn-block">
									Mark all as Unseen
								</button>
								<button class="btn btn-primary btn-block">
									Manage Mute Notification
								</button>
								<button class="btn btn-primary btn-block">
									Mute all Notification
								</button>
							</div>
						</div>
					</div> -->
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { notificationMixin } from "../../mixins/notification";
import Notification from "@/components/notification/Notification.vue";
import JwtService from "@/services/jwt.service";
import ApiService from "@/services/api.service";
import { openModalRoute } from "@/plugins/modal/modal.mixin";
export default {
  name: "NotificationPage",
  components: {
    Notification,
  },
  mixins: [notificationMixin],
  data() {
    return {
      isLoading: false,
      user: {},
      is_verified: 1,
      error: null,
      teamId: null,
      notiType: "all",
    };
  },
  computed: {
    filteredNotifications() {
      if (this.notiType == "all") {
        return this.notifications;
      } else {
        return this.notifications.filter((item) => item.seen == this.notiType);
      }
    },
    notifications() {
      return this.$store.state.notification.notifications;
    },
  },
  created() {
    this.getActiveTeamId();
  },
  methods: {
    getActiveTeamId() {
      if (!JwtService.getTeamIDAppWide()) {
        this.isLoading = true;
        setTimeout(() => {
          this.isLoading = false;
          openModalRoute(this, "manage_team_redirect");
        }, 2000);
      } else {
        this.$store.state.notification.notifications.forEach(item => {
          item.seen = 1;
        });

        this.markAllAsRead();
        this.$store.state.notification.instantNotifications = [];
      }
    },
    async loadNotifications() {
      this.isLoading = true;
      try {
        ApiService.get("v1/list-notification")
          .then((response) => {
            this.notifications = response.data.data;
            this.isLoading = false;
          })
          .catch((e) => {
            console.log(e);
            this.isLoading = false;
          });
      } catch (error) {
        this.error = error.message || "Something went wrong! Try again!";
        console.log(this.error);
        this.isLoading = false;
      }
    },
    changeTeam(data) {
      this.teamId = data;
    },
  },
};
</script>

<style scoped lang="scss">
@import "@/styles/base/_variables.scss";
.main-content-wrapper {
  margin-top: 0px !important;
  padding: 12px 15px 12px 15px; 
  .side-bar {
    max-width: 250px;
  }
  @media (max-width: 1024px) {
    flex-wrap: wrap;
    .side-bar {
      width: 30%;
      flex: 0 0 30%;
    }
  }

  @media (max-width: 767px) {
    max-width: 100%;
    .side-bar {
      width: 30%;
      flex: 0 0 30%;
    }
  }

  @media (max-width: 541px) {
    .side-bar {
      width: 120px;
      flex: 0 0 10%;
    }
  }

  @media (max-width: 415px) {
    .side-bar {
      // max-width: 128px;
      width: auto;
      flex: 0 0 10%;
    }
  }

  .main-content-1 {
    flex: 0 0 100%;
  }
  // .main-content-2 {
  // 	margin: 15px;
  // 	width: 300px;
  // 	height: 80vh;
  // 	.notifications-sidebar {
  // 		padding: 10px;
  // 		height: 80vh;
  // 		.filter-notification {
  // 			height: 50%;
  // 			padding: 15px;
  // 			h6 {
  // 				text-align: center;
  // 			}
  // 		}
  // 		.manage-notification {
  // 			height: 50%;
  // 			padding: 15px;
  // 			h6 {
  // 				text-align: center;
  // 			}
  // 		}
  // 	}
  // 	@media (max-width: 1025px) {
  // 		display: none;
  // 	}
  // }

  // @media (max-width: 767px) {
  // 	.footer-portion{
  // 		display: none;
  // 	}
  // }
}
.d-sm-none {
  display: none;
  @media (min-width: 992px) {
    display: block;
  }
}
.d-md-block {
  display: none;
  @media (min-width: 992px) {
    display: flex !important;
    width: 100%;
  }
}
.notification-page-height {
  height: calc(100vh - 165px);
  overflow-y: unset;
  @media (min-width: 992px) {
    height: calc(100vh - 175px);
    overflow-y: auto;
  }
}
.read-btn {
  margin-top: 6px;
  @media (min-width: 768px) {
    margin-top: 0;
  }
}
.all-button {
  color: $bg-primary;
  cursor: pointer;
  &:hover {
    text-decoration: underline;
  }
}

.unread-button {
  color: $bg-brand;
  cursor: pointer;
  &:hover {
    text-decoration: underline;
  }
}
</style>
