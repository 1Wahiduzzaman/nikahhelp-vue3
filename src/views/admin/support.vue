<template>
  <div class="panel-container">
    <div class="chat-wrapper py-4">
      <div class="chat-left">
        <div class="chat-title d-chat-title">Support chat</div>
        <div class="chat-search">
          <div class="form-group has-search">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 42.02 40.76">
              <g id="Layer_2" data-name="Layer 2">
                <g id="draw_boxes" data-name="draw boxes">
                  <path fill="#cccccc" class="cls-1"
                        d="M41.67,35.89A1.33,1.33,0,0,0,41.58,34L30.34,23.79a1.34,1.34,0,0,0-1.88.09l-4,4.44a1.34,1.34,0,0,0,.09,1.88L35.78,40.41a1.32,1.32,0,0,0,1.88-.09Z"/>
                  <path fill="#cccccc" class="cls-1"
                        d="M14.72,0A14.72,14.72,0,1,1,9.91.81,14.73,14.73,0,0,1,14.72,0m0,4a10.82,10.82,0,0,0-3.51.59h0A10.73,10.73,0,1,0,14.72,4Z"/>
                </g>
              </g>
            </svg>
            <input type="text" class="form-control" placeholder="Search..." v-model="conv_search_key">
          </div>
        </div>
        <div class="chat-item-wrapper border-top">
          <div class="tab-content">
            <chat-list-item class="cursor-pointer border-bottom py-2" v-for="(item, index) in histories" :item="item" @click.native="loadIndividualChatHistory(item)" :key="index" />
          </div>
        </div>
      </div>
      <div class="chat-right position-relative" v-if="chatheadopen">
<!--        <h4 class="cursor-pointer position-absolute btn-short-back">&#8592;</h4>-->
        <div class="header clearfix">
          <div class="left">
            <div class="top align-items-center">
              <div class="item-img">
                <img src="../../assets/info-img.png" alt="info image">
              </div>
              <div class="chat-info">
                <div class="chat-name">{{ chatheadopen && chatheadopen.user ? chatheadopen.user.full_name : '' }}</div>
              </div>
            </div>
          </div>
          <div class="right">
            <a-dropdown>
              <a class="ant-dropdown-link py-2" @click="e => e.preventDefault()">
                <MoreOutlined class="fs-30 font-weight-bolder rotate-90 br-50 bg-c9 color-primary"/>
              </a>
              <a-menu slot="overlay" class="text-center px-2">
                <a-menu-item class="border-bottom">
                  <a class="fs-12 color-primary">Mute this chat</a>
                </a-menu-item>
                <a-menu-item class="border-bottom">
                  <a class="fs-12 color-primary">Mark as read</a>
                </a-menu-item>
                <a-menu-item class="border-bottom">
                  <a class="fs-12 color-primary">Add to chat history</a>
                </a-menu-item>
                <a-menu-item class="border-bottom">
                  <a class="fs-12 color-primary">Archive chat</a>
                </a-menu-item>
                <a-menu-item>
                  <a class="text-danger fs-12">Delete chat</a>
                </a-menu-item>
              </a-menu>
            </a-dropdown>
          </div>
        </div>
        <div class="chat-area">
          <div class="position-relative">
            <div class="chat-messages py-4 pr-1" id="chat-messages">
              <div class="position-relative" v-for="(item, cIndex) in chats" :key="item.id" :id="chats.length === cIndex + 1 ? 'messagesid' : ''">
                <div class="chat-message-right pb-4 position-relative"
                     v-if="(parseInt(item.senderId) == parseInt(getAuthUserId))"
                     :class="{'conv-mb': chats.length !== cIndex + 1}" >
                  <div class="text-right">
                    <img src="@/assets/icon/support-secondary.svg" class="rounded-circle mr-1" alt="Chris Wood" width="40" height="40">
                  </div>
                  <div class="flex-shrink-1 py-2 px-3 mr-3 bg-me text-white br-10 white-space-pre" v-html="item.body"></div>
                  <div class="text-muted small text-nowrap mt-2 position-absolute msg-right-created-at">{{ messageCreatedAt(item.created_at) }}</div>
                </div>

                <div class="chat-message-left pb-4 position-relative"
                     v-else
                     :class="{'conv-mb': chats.length !== cIndex + 1}">
                  <div class="text-left">
                    <img :src="getAuthUser && getAuthUser.per_main_image_url ? getAuthUser.per_main_image_url : avatarSrc" class="rounded-circle mr-1" alt="Sharon Lessman" width="40" height="40">
                  </div>
                  <div class="flex-shrink-1 bg-light py-2 px-3 ml-3 br-10 white-space-pre" v-html="item.body"></div>
                  <div class="text-muted small text-nowrap mt-2 position-absolute msg-left-created-at">{{ messageCreatedAt(item.created_at) }}</div>
                </div>
              </div>

            </div>
          </div>
          <div class="footer">
            <div class="footer-bottom">
              <form action="#" @submit.prevent="sendMsg">
                <div class="left flex justify-content-end align-items-end">
                  <div class="message-box">
                    <a-tooltip>
                      <template #title>
                        Coming soon
                      </template>
                      <button class="btn-emoji px-2" title="Coming soon">&#128528;</button>
                    </a-tooltip>
                    <textarea name="message" id="" cols="30" rows="4" placeholder="Enter message..."
                              v-model="message" @keydown.enter.exact.prevent="sendMsg($event)"></textarea>
                    <div class="position-absolute msgbox-right">
                      <div class="flex">
                        <a-tooltip>
                          <template #title>
                            Coming soon
                          </template>
                          <button><img src="../../assets/icon/microphone.png" alt="icon" class="mr-2 microphone" /></button>
                        </a-tooltip>
                        <a-tooltip>
                          <template #title>
                            Coming soon
                          </template>
                          <button>
                            <FileImageOutlined class="color-primary"/>
                          </button>
                        </a-tooltip>
                      </div>
                    </div>
                  </div>
                  <button class="btn btn-primary btn-submit js-msg-send">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 25.68 18.77">
                      <g id="Layer_2" data-name="Layer 2">
                        <g id="middle" fill="white">
                          <polygon class="cls-1" points="8.6 12.29 7.06 13.34 6.86 11.05 8.6 12.29"/>
                          <polygon class="cls-1" points="0 6.23 25.68 0 17.75 18.77 0 6.23"/>
                          <polygon class="cls-2" points="6.86 11.05 24.95 0.57 8.6 12.29 6.86 11.05"/>
                        </g>
                      </g>
                    </svg>
                    Send
                  </button>
                </div>
              </form>
            </div>
          </div>
        </div>
      </div>
<!--      <div class="chat-right">-->
<!--        <div class="flex justify-content-center align-items-center empty-height">-->
<!--          <h4 class="fs-20 flex flex-column align-items-center justify-content-center">Select a conversation & start the chat</h4>-->
<!--        </div>-->
<!--      </div>-->
    </div>
  </div>
</template>

<script>
import ChatListItem from "../../components/support/ChatListItem.vue";
import ApiService from '@/services/api.service';
import {format} from "timeago.js";

import {MoreOutlined, FileImageOutlined} from '@ant-design/icons-vue';

export default {
  name: 'Support',
  sockets: {
    connect: function () {
      console.log('socket connected')
    },
    ping: function (data) {
      console.log('this method was fired by the socket server. eg: io.emit("customEmit", data)')
    }
  },
  data() {
    return {
      conv_search_key: '',
      msg_text: '',
      histories: [],
      chats: [],
      chatheadopen: null,
      avatarSrc: "https://www.w3schools.com/w3images/avatar2.png",
      message: '',
      online_users: [],
      ws: null
    };
  },
  computed: {
    getAuthUserId() {
      let loggedUser = JSON.parse(localStorage.getItem('user'));
      if (loggedUser) {
        return loggedUser.id;
      }
      return null;
    },
    getAuthUser() {
      let loggedUser = JSON.parse(localStorage.getItem('user'));
      if (loggedUser) {
        return loggedUser;
      }
      return null;
    },
  },
  components: {
    ChatListItem,
    MoreOutlined,
    FileImageOutlined
  },
  created() {
    this.loadLists();
  },
  methods: {
    messageCreatedAt(time) {
      return format(time);
    },
    async loadLists() {
      let {data} = await ApiService.get('/v1/support-chat-list').then(res => res.data);
      if(data && data.chat_list) {
        this.histories = data.chat_list;
      }
    },
    async loadIndividualChatHistory(item) {
      this.chatheadopen = item;
      this.chatheadopen.last_message.seen = 1;
      let {data} = await ApiService.post('/v1/individual-support-user-chat-history', {
        chat_id: item.last_message.chat_id
      }).then(res => res.data);
      this.chats = data.map(msg => {
        msg.senderId = msg.sender ? msg.sender.id : '';
        msg.receiverId = msg.receiver ? msg.receiver.id : '';
        return msg;
      });
    },
    async sendMsg(e) {
      console.log(e);
      if(this.chatheadopen) {
        let loggedUser = JSON.parse(localStorage.getItem('user'));
        let payload = {
          sender: loggedUser.id,
          receiver: this.chatheadopen.user.id.toString(),
          to: this.chatheadopen.user.id.toString(),
          message: this.message,
          user: loggedUser,
          chat_id: this.chatheadopen.chat_id,
          support: true,
          last_message: {
            body: this.message,
            created_at: new Date(),
            sender: loggedUser,
            senderId: loggedUser.id,
            receiver: this.chatheadopen.user,
            receiverId: this.chatheadopen.user.id,
            seen: 0,
            chat_id: this.chatheadopen.chat_id,
          }
        };
        // this.$socket.emit('send_message', payload);
        this.ws.send(JSON.stringify({
          action: 'send_message',
          data: payload,
          component: 'support'
        }));
        
        this.chats.push(payload.last_message);
        this.message = '';
        await ApiService.post('/v1/support-send-message', payload).then(response => {
          console.log(response);
        });
      }
    }
  },
  mounted() {
    let loggedUser = JSON.parse(localStorage.getItem('user'));
    let self = this;
    if(loggedUser) {
      this.ws = new WebSocket(`${import.meta.env.VITE_CHAT_SERVER}`);

      // this.$socket.emit('ping', {user_id: loggedUser.id});

      this.ws.onopen = function() {
        self.ws.send(JSON.stringify({
          action: 'ping',
          user_id: loggedUser.id,
          component: 'support'
        }));
      };

      this.ws.onmessage = function($event) {
        let res = JSON.parse($event.data);

        if(res.event == 'ping_success') {
          if (res && res.data.online_users) {
            self.online_users = res.data.online_users;
            self.$store.state.chat.online_users = res.data.online_users;
          }
        } else if(res.event == 'receive_message') {
          if(res.data.support) {
            self.loadLists();
            let hasChat = self.histories.find(history => history && history.last_message && history.last_message.chat_id);
            if(hasChat) {
              let chatId = hasChat.last_message.chat_id;
              hasChat.last_message = res.data.last_message;
              hasChat.last_message.chat_id = chatId;
              if(self.chatheadopen && self.chatheadopen.user.id == res.data.sender) {
                self.chats.push(res.data.last_message);
              }
            }
          }
        }
      };
    }
  },
  watch: {
    chats: function(val) {
      // console.log(val);
      setTimeout(() => {
        const messages = document.getElementById('chat-messages');
        const messagesid = document.getElementById('messagesid');
        messages.scrollTop = messagesid.offsetTop - 10;
      });
    }
  }
};
</script>

<style scoped lang="scss">
// start css for chat
.panel-container {
  //display: flex;
  //flex-direction: row;
  //justify-content: space-around;
  height: calc(100vh - 150px);
  overflow: hidden;
  border: 2px solid #ddd;
  border-radius: 15px;
  background: #ffffff 0% 0% no-repeat padding-box;
  box-shadow: 0px 10px 30px #fff;
  margin: 20px;
  opacity: 1;

}
.msgbox-right {
  top: 10px;
  right: 10px;
}
.chat-hide {
  display: none;
}

.chat-show {
  display: block;
}

.d-chat-title {
  display: none;
}

@media (min-width: 992px) {
  .chat-hide {
    display: block;
  }
  .d-chat-title {
    display: block;
  }
}

.clearfix {
  &::after {
    content: "";
    clear: both;
    display: table;
  }
}

.chat-wrapper {
  margin: auto;
  padding: 20px;
  width: 100%;
  border-radius: 10px;
  background-color: #ffffff;
  box-shadow: 0px 0px 10px 1px rgba(63, 6, 17, 0.3);
  @media (max-width: 991px) {
    padding: 10px;
  }

  &::after {
    content: "";
    display: table;
    clear: both;
    overflow: hidden;

  }

  .chat-left {
    float: left;
    //width: 400px;
    width: 100%;
    max-width: 100%;
    padding-right: 5px;
    //min-height: 600px;
    //height: calc(100vh - 195px);
    border-right: 2px solid #e5e5e5;
    @media (max-width: 913px) {
      .category-item {
        padding-bottom: 0px;
        margin-bottom: -20px;

        &::after {
          margin-bottom: 20px;
        }
      }
    }

    @media (min-width: 992px) {
      width: 400px;
    }

    @media (max-width: 913px) {
      .category-name {
        display: none;
        padding-bottom: 0px;
        margin-bottom: -100px;

      }
    }

    @media (max-width: 767px) {
      width: 100%;
      padding-right: 0;
      border-right: 0;
    }

    .chat-title {
      font-size: 30px;
      padding-left: 10px;
      margin-bottom: 15px;
    }

    .chat-search {
      position: relative;
      margin: 0 10px 10px;

      .has-search {
        svg {
          width: 24px;
          position: absolute;
          top: 50%;
          left: 10px;
          transform: translate(0, -50%);
        }
      }

      .has-search .form-control {
        padding-left: 50px;

        border: 0;
        border-radius: 10px;
        background: #f2f2f2;

        &::after {
          content: "";
          position: absolute;
          width: 100%;
          height: 100%;
          border-radius: 50%;
          top: 0;
          right: 0;
          background-color: #f2f2f2;

        }
      }

      .has-search .form-control-feedback {
        position: absolute;
        z-index: 2;
        display: block;
        width: 2.375rem;
        height: 2.375rem;
        line-height: 2.375rem;
        text-align: center;
        pointer-events: none;
        color: #aaa;
      }
    }

    .chat-category {
      display: flex;
      padding-bottom: 4px;
      padding-top: 2px;
      margin: 0 10px 20px;
      justify-content: space-between;
      border-bottom: 2px solid #f2f2f2;
      //border-top: 2px solid #f2f2f2;
      @media (max-width: 991px) {
        padding-bottom: 10px;
      }

      nav {
        //width: calc(100% - 44px);
        width: 100%;

        .nav-tabs {
          border: 0;
          display: flex;
          justify-content: space-between;
          //align-items: center;
          .nav-link {
            margin-bottom: -2px;
            border: 0;
            padding-bottom: 2px;
            @media (max-width: 991px) {
              padding: 0 5px;
            }
            @media (max-width: 767px) {
              padding: 0 12px;
            }
            @media (max-width: 375px) {
              padding: 0 3px;
            }

            .category-item {
              display: inline-block;
              text-align: center;
              position: relative;

              a {
                min-height: 48px;
                display: block;

                svg {
                  width: 26px;
                  height: 26px;
                  fill: #cccccc;
                }
              }

              img {
                width: 26px;
              }

              .category-name {
                color: #cccccc;
                font-size: 14px;
                font-weight: bold;
                @media (max-width: 990px) {
                  font-size: 8px;
                  font-weight: normal;
                }
              }


              .countOfChat {
                background-color: #e51f76;
                float: right;
                position: absolute;
                z-index: 1;
                padding-bottom: 10px;
                border-radius: 50%;
                font-size: 11px;
                color: #ffffff;
                width: 20px;
                text-align: center;
                height: 20px;
                padding-top: 2px;
                margin-left: -2px;
                @media (max-width: 767px) {
                  font-size: 9px;
                  width: 16px;
                  height: 16px;
                }
              }

              .dropdown-menu {
                z-index: 1061;
                margin-top: 7px;
                padding: 10px 15px;
                box-shadow: 0 0 3px 2px #ddd;
                transform: none !important;
                right: 20px;
                left: auto !important;
                top: 27px !important;

                .dropdown-item {
                  padding: 3px 0;
                  margin-bottom: 2px;
                  text-align: center;
                  font-size: 12px;
                  border-radius: 3px;
                  color: #3a3092;
                  background-color: #e6e6e6;

                  &:hover {
                    color: #ffffff;
                    background-color: #e51f76;
                  }
                }
              }

              a {
                p {
                  margin: 0;
                }
              }
            }

            &.active {
              .category-item {
                &::after {
                  content: "";
                  width: 100%;
                  border-bottom: 2px solid #6059a7;
                  height: 2px;
                  background-color: red;
                  position: absolute;
                  left: 0;
                  bottom: -6px;
                }

                svg {
                  fill: #6059a7;
                }

                .category-name {
                  color: #6059a7;
                }
              }
            }
          }
        }
      }

      .info-button {
        position: relative;

        button {
          &::after {
            border: 0;
            width: 12px;
            height: 20px;
            margin-top: 7px;
            background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAcAAAAUCAIAAADOcrCxAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyNpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTQ4IDc5LjE2NDAzNiwgMjAxOS8wOC8xMy0wMTowNjo1NyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIDIxLjAgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkNFMkU1NjBEQkVFMTExRUJCREYzRjRFMkMxMERCRjAwIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkNFMkU1NjBFQkVFMTExRUJCREYzRjRFMkMxMERCRjAwIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6Q0UyRTU2MEJCRUUxMTFFQkJERjNGNEUyQzEwREJGMDAiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6Q0UyRTU2MENCRUUxMTFFQkJERjNGNEUyQzEwREJGMDAiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz7zerWhAAABDElEQVR42mL4DwZfv/wEkt+//4ZwWRgYGG7ffH3tyksGMPAP1gGSTN++/oILAcHGtVf+/fvPxMXNxoAKmJgYmX78+IMsxMYOMpOJg4PFwEgaLqRvKAVkMAJtBFIgs5gYEYZAqJcvPgPJVy+/QLggUy6ce/rw/nsIX1VdVEtHnOnTxx9wIYjbf/38w/Tz5180l/389ZdJWIQLWYiPn4OXl50JaLW1naKgEEhORJTH2FQG4TJ07wExUOLm9Vcgq269gYiC1B47/OD1K6hLZWQFjM1kmN69+wYXAoInjz98//ab6f8/dEP//f8PcpmAACdcSEycl5ubDWQuMDBvXHsJ9KGSijDQu8zMTAABBgDuN39O859xpAAAAABJRU5ErkJggg==) no-repeat right center;
          }
        }

        .dropdown-menu {
          z-index: 1061;
          margin-top: 7px;
          padding: 10px 15px;
          box-shadow: 0 0 3px 2px #ddd;
          transform: none !important;
          right: 0px;
          left: auto !important;
          top: 27px !important;

          .dropdown-item {
            padding: 3px 0;
            margin-bottom: 2px;
            text-align: center;
            font-size: 12px;
            border-radius: 3px;
            color: #3a3092;
            background-color: #e6e6e6;

            &:hover {
              color: #ffffff;
              background-color: #e51f76;
            }
          }
        }
      }

      .pending-message {
        display: inline-block;
        background-color: #e51f76;
        border-radius: 50%;
        font-size: 11px;
        color: #ffffff;
        width: 20px;
        text-align: center;
        height: 20px;
        padding-top: 2px;
        // float: right;
        @media (max-width: 767px) {
          font-size: 9px;
          width: 16px;
          height: 16px;
        }
      }
    }

    .chat-item-wrapper {
      overflow-y: auto;

      .chat-item {
        padding-top: 15px;
        padding-bottom: 10px;
        border-bottom: 1px solid #ececec;

        .top {
          display: flex;

          .item-img {
            height: 60px;
            width: 50px;
            position: relative;
            @media (max-width: 991px) {
              height: 50px;
              width: 35px;
            }

            img {
              width: 50px;
              border: 2px solid #3ab54a;
              border-radius: 50%;
            }

            span {
              right: 1px;
              bottom: 11px;
              width: 15px;
              height: 15px;
              border-radius: 50%;
              position: absolute;
              background-color: #3ab54a;
              border: 2px solid #ffffff;
            }
          }

          .chat-info {
            max-width: 72%;
            padding-left: 15px;
            @media (max-width: 991px) {
              max-width: 66%;
            }

            .chat-group {
              background-color: #b1acd4;
              padding: 0px 5px;
              margin-bottom: 2px;
              width: auto;
              float: left;
              clear: both;
              border-radius: 5px;
              font-size: 12px;
              color: #ffffff;
            }

            .chat-name {
              font-size: 14px;
              color: #4e4e4e;
              font-weight: bold;
              clear: both;
            }

            .last-chat {
              color: #a7a7a7;
              width: 100%;
              overflow: hidden;
              text-overflow: ellipsis;
              white-space: nowrap;

            }
          }

          .status-info {
            display: flex;
            padding-top: 13px;
            align-items: flex-start;

            .status {
              width: 16px;
              height: 16px;
              background: #e61e76;
              border-radius: 50%;
              border: 2px solid #cacaca;
              margin-top: 10px;
              margin-right: 5px;
            }

            .info-button {
              position: relative;

              button {
                &::after {
                  border: 0;
                  width: 12px;
                  height: 20px;
                  margin-top: 7px;
                  background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAcAAAAUCAIAAADOcrCxAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyNpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTQ4IDc5LjE2NDAzNiwgMjAxOS8wOC8xMy0wMTowNjo1NyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIDIxLjAgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkNFMkU1NjBEQkVFMTExRUJCREYzRjRFMkMxMERCRjAwIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkNFMkU1NjBFQkVFMTExRUJCREYzRjRFMkMxMERCRjAwIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6Q0UyRTU2MEJCRUUxMTFFQkJERjNGNEUyQzEwREJGMDAiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6Q0UyRTU2MENCRUUxMTFFQkJERjNGNEUyQzEwREJGMDAiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz7zerWhAAABDElEQVR42mL4DwZfv/wEkt+//4ZwWRgYGG7ffH3tyksGMPAP1gGSTN++/oILAcHGtVf+/fvPxMXNxoAKmJgYmX78+IMsxMYOMpOJg4PFwEgaLqRvKAVkMAJtBFIgs5gYEYZAqJcvPgPJVy+/QLggUy6ce/rw/nsIX1VdVEtHnOnTxx9wIYjbf/38w/Tz5180l/389ZdJWIQLWYiPn4OXl50JaLW1naKgEEhORJTH2FQG4TJ07wExUOLm9Vcgq269gYiC1B47/OD1K6hLZWQFjM1kmN69+wYXAoInjz98//ab6f8/dEP//f8PcpmAACdcSEycl5ubDWQuMDBvXHsJ9KGSijDQu8zMTAABBgDuN39O859xpAAAAABJRU5ErkJggg==) no-repeat right center;
                }
              }

              .dropdown-menu {
                z-index: 1061;
                margin-top: 7px;
                padding: 10px 15px;
                box-shadow: 0 0 3px 2px #ddd;
                transform: none !important;
                right: 0px;
                left: auto !important;
                top: 27px !important;

                .dropdown-item {
                  padding: 3px 0;
                  margin-bottom: 2px;
                  text-align: center;
                  font-size: 12px;
                  border-radius: 3px;
                  color: #3a3092;
                  background-color: #e6e6e6;

                  &:hover {
                    color: #ffffff;
                    background-color: #e51f76;
                  }
                }
              }
            }
          }
        }

        .bottom {
          display: flex;
          justify-content: space-between;
          position: relative;
          font-size: 12px;

          .typing {
            color: #776db2;
            padding-left: 65px;
            @media (max-width: 991px) {
              padding-left: 51px;
            }
          }

          .chat-time {
            color: #776db2;
            padding-right: 24px;
          }
        }

        &.offline {
          .top {
            .item-img {
              img {
                border-color: #fa4943;
              }

              span {
                background-color: #fa4943;
              }
            }
          }
        }
      }
    }
  }

  .chat-right {
    float: left;
    padding-left: 20px;
    width: 100%;
    //width: calc(100% - 400px);
    @media (min-width: 992px) {
      width: calc(100% - 400px)
    }
    //@media (max-width: 991px) {
    //  width: calc(100% - 302px);
    //}
    //@media (max-width: 767px) {
    //  width: 100%;
    //  padding-left: 0;
    //}

    .header {
      border-bottom: 1px solid #d9d9d9;
      padding-bottom: 10px;
      margin-bottom: 20px;
      @media (max-width: 991px) {
        position: relative;
      }

      .left {
        float: left;
        min-width: 277px;

        .top {
          display: flex;

          .item-img {
            width: 50px;
            position: relative;
            @media (max-width: 767px) {
              width: 35px;
              height: 38px;
            }

            img {
              width: 50px;
              border: 2px solid #3ab54a;
              border-radius: 50%;
            }

            span {
              right: 2px;
              bottom: -3px;
              width: 15px;
              height: 15px;
              border-radius: 50%;
              position: absolute;
              background-color: #3ab54a;
              border: 2px solid #ffffff;
            }
          }

          .chat-info {
            max-width: 72%;
            padding-left: 15px;
            @media (max-width: 767px) {
              max-width: 57%;
            }

            .chat-group {
              background-color: #b1acd4;
              padding: 0px 5px;
              margin-bottom: 2px;
              width: auto;
              float: left;
              clear: both;
              border-radius: 5px;
              font-size: 12px;
              color: #ffffff;
            }

            .chat-name {
              font-size: 14px;
              color: #4e4e4e;
              font-weight: bold;
              clear: both;
            }

            .last-chat {
              color: #6059a7;
              font-size: 12px;
              width: 100%;
              overflow: hidden;
              text-overflow: ellipsis;
              white-space: nowrap;

            }
          }

          .status-info {
            display: flex;
            padding-top: 13px;
            align-items: flex-start;

            .status {
              width: 16px;
              height: 16px;
              background: #e61e76;
              border-radius: 50%;
              border: 2px solid #cacaca;
              margin-top: 10px;
              margin-right: 5px;
            }

            .info-button {
              position: relative;

              button {
                &::after {
                  border: 0;
                  width: 12px;
                  height: 20px;
                  margin-top: 7px;
                  background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAcAAAAUCAIAAADOcrCxAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyNpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTQ4IDc5LjE2NDAzNiwgMjAxOS8wOC8xMy0wMTowNjo1NyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIDIxLjAgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkNFMkU1NjBEQkVFMTExRUJCREYzRjRFMkMxMERCRjAwIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkNFMkU1NjBFQkVFMTExRUJCREYzRjRFMkMxMERCRjAwIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6Q0UyRTU2MEJCRUUxMTFFQkJERjNGNEUyQzEwREJGMDAiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6Q0UyRTU2MENCRUUxMTFFQkJERjNGNEUyQzEwREJGMDAiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz7zerWhAAABDElEQVR42mL4DwZfv/wEkt+//4ZwWRgYGG7ffH3tyksGMPAP1gGSTN++/oILAcHGtVf+/fvPxMXNxoAKmJgYmX78+IMsxMYOMpOJg4PFwEgaLqRvKAVkMAJtBFIgs5gYEYZAqJcvPgPJVy+/QLggUy6ce/rw/nsIX1VdVEtHnOnTxx9wIYjbf/38w/Tz5180l/389ZdJWIQLWYiPn4OXl50JaLW1naKgEEhORJTH2FQG4TJ07wExUOLm9Vcgq269gYiC1B47/OD1K6hLZWQFjM1kmN69+wYXAoInjz98//ab6f8/dEP//f8PcpmAACdcSEycl5ubDWQuMDBvXHsJ9KGSijDQu8zMTAABBgDuN39O859xpAAAAABJRU5ErkJggg==) no-repeat right center;
                }
              }

              .dropdown-menu {
                z-index: 1061;
                margin-top: 7px;
                padding: 10px 15px;
                box-shadow: 0 0 3px 2px #ddd;
                transform: none !important;
                right: 0px;
                left: auto !important;
                top: 27px !important;

                .dropdown-item {
                  padding: 3px 0;
                  margin-bottom: 2px;
                  text-align: center;
                  font-size: 12px;
                  border-radius: 3px;
                  color: #3a3092;
                  background-color: #e6e6e6;

                  &:hover {
                    color: #ffffff;
                    background-color: #e51f76;
                  }
                }
              }
            }
          }
        }
      }

      .middle {
        float: left;
        @media (max-width: 991px) {
          margin-left: 66px;
        }
        @media (max-width: 767px) {
          clear: both;
        }

        .chat-group {
          background-color: #8882be;
          padding: 0 5px;
          font-size: 11px;
          color: #ffffff;
          width: auto;
          border-radius: 5px;
        }
      }

      .right {
        float: right;
        @media (max-width: 991px) {
          position: absolute;
          right: 0;
        }

        .chat-search {
          float: left;
          position: absolute;
          right: 50px;
          margin: 0 10px 0 0;
          @media (max-width: 991px) {
            right: 20px;
          }
          @media (max-width: 767px) {
            right: 10px;
          }

          .has-search {
            position: relative;
            padding-right: 30px;
            background-color: #eceaf5;
            border: 0;
            border-radius: 25px;
            height: 40px;
            transition: 0.5s;
            padding: 0;

            svg {
              width: 21px;
              position: absolute;
              top: 50%;
              right: 10px;
              z-index: 0;
              transform: translate(0, -50%);
            }
          }

          .has-search .form-control {
            padding-right: 30px;
            border: 0;
            width: 40px;
            height: 40px;
            transition: 0.5s;
            padding: 0 15px;
            position: relative;
            z-index: 1;
            background: transparent;
            box-shadow: none;
            cursor: pointer;

            &:focus {
              width: 230px;
              transition: 0.5s;
              border-radius: 7px;
            }
          }

          .has-search .form-control-feedback {
            position: absolute;
            z-index: 2;
            display: block;
            width: 2.375rem;
            height: 2.375rem;
            line-height: 2.375rem;
            text-align: center;
            pointer-events: none;
            color: #aaa;
          }
        }

        .info-button {
          float: left;
          position: relative;

          button {
            &::after {
              border: 0;
              width: 12px;
              height: 20px;
              margin-top: 7px;
              background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAcAAAAUCAIAAADOcrCxAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyNpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTQ4IDc5LjE2NDAzNiwgMjAxOS8wOC8xMy0wMTowNjo1NyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIDIxLjAgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkNFMkU1NjBEQkVFMTExRUJCREYzRjRFMkMxMERCRjAwIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkNFMkU1NjBFQkVFMTExRUJCREYzRjRFMkMxMERCRjAwIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6Q0UyRTU2MEJCRUUxMTFFQkJERjNGNEUyQzEwREJGMDAiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6Q0UyRTU2MENCRUUxMTFFQkJERjNGNEUyQzEwREJGMDAiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz7zerWhAAABDElEQVR42mL4DwZfv/wEkt+//4ZwWRgYGG7ffH3tyksGMPAP1gGSTN++/oILAcHGtVf+/fvPxMXNxoAKmJgYmX78+IMsxMYOMpOJg4PFwEgaLqRvKAVkMAJtBFIgs5gYEYZAqJcvPgPJVy+/QLggUy6ce/rw/nsIX1VdVEtHnOnTxx9wIYjbf/38w/Tz5180l/389ZdJWIQLWYiPn4OXl50JaLW1naKgEEhORJTH2FQG4TJ07wExUOLm9Vcgq269gYiC1B47/OD1K6hLZWQFjM1kmN69+wYXAoInjz98//ab6f8/dEP//f8PcpmAACdcSEycl5ubDWQuMDBvXHsJ9KGSijDQu8zMTAABBgDuN39O859xpAAAAABJRU5ErkJggg==) no-repeat right center;
            }
          }

          .dropdown-menu {
            z-index: 1061;
            margin-top: 7px;
            padding: 10px 15px;
            box-shadow: 0 0 3px 2px #ddd;
            transform: none !important;
            right: 0px;
            left: auto !important;
            top: 27px !important;

            .dropdown-item {
              padding: 3px 0;
              margin-bottom: 2px;
              text-align: center;
              font-size: 12px;
              border-radius: 3px;
              color: #3a3092;
              background-color: #e6e6e6;

              &:hover {
                color: #ffffff;
                background-color: #e51f76;
              }
            }
          }
        }
      }
    }

    .chat-area {
      //min-height: 620px;
      display: flex;
      flex-direction: column;
      position: relative;

      .chat-box {
        height: 545px;
        overflow-y: auto;
        display: flex;
        flex-direction: column-reverse;
        @media (max-width: 991px) {
          height: 500px;
        }

        .chats {
          display: flex;
          margin-bottom: 20px;

          .item-img {
            width: 30px;
            height: 30px;
            position: relative;
            margin-right: 10px;

            img {
              width: 50px;
              border: 2px solid #3ab54a;
              border-radius: 50%;
            }

            span {
              right: 1px;
              right: 1px;
              bottom: -2px;
              width: 9px;
              height: 9px;
              border-radius: 50%;
              position: absolute;
              background-color: #3ab54a;
              border: 1px solid #ffffff;
            }
          }

          .message {
            max-width: 60%;
            @media (max-width: 991px) {
              max-width: 76%;
            }

            .msg-text {
              font-size: 14px;
              background: #eceaf5;
              border-radius: 5px;
              padding: 5px;
              margin-bottom: 5px;
            }

            .date-time {
              font-size: 10px;
              margin: 0;

              span {
                background-color: #6159a7;
                color: #ffffff;
                padding: 0 6px;
                border-radius: 4px;
              }
            }
          }

          .info-button {
            position: relative;

            button {
              &::after {
                border: 0;
                width: 12px;
                height: 20px;
                margin-top: 7px;
                background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAcAAAAUCAIAAADOcrCxAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyNpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTQ4IDc5LjE2NDAzNiwgMjAxOS8wOC8xMy0wMTowNjo1NyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIDIxLjAgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkNFMkU1NjBEQkVFMTExRUJCREYzRjRFMkMxMERCRjAwIiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkNFMkU1NjBFQkVFMTExRUJCREYzRjRFMkMxMERCRjAwIj4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6Q0UyRTU2MEJCRUUxMTFFQkJERjNGNEUyQzEwREJGMDAiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6Q0UyRTU2MENCRUUxMTFFQkJERjNGNEUyQzEwREJGMDAiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz7zerWhAAABDElEQVR42mL4DwZfv/wEkt+//4ZwWRgYGG7ffH3tyksGMPAP1gGSTN++/oILAcHGtVf+/fvPxMXNxoAKmJgYmX78+IMsxMYOMpOJg4PFwEgaLqRvKAVkMAJtBFIgs5gYEYZAqJcvPgPJVy+/QLggUy6ce/rw/nsIX1VdVEtHnOnTxx9wIYjbf/38w/Tz5180l/389ZdJWIQLWYiPn4OXl50JaLW1naKgEEhORJTH2FQG4TJ07wExUOLm9Vcgq269gYiC1B47/OD1K6hLZWQFjM1kmN69+wYXAoInjz98//ab6f8/dEP//f8PcpmAACdcSEycl5ubDWQuMDBvXHsJ9KGSijDQu8zMTAABBgDuN39O859xpAAAAABJRU5ErkJggg==) no-repeat right center;
              }
            }

            .dropdown-menu {
              z-index: 1061;
              margin-top: 7px;
              padding: 10px 15px;
              box-shadow: 0 0 3px 2px #ddd;
              transform: none !important;
              right: 0px;
              left: auto;
              top: 27px !important;

              .dropdown-item {
                padding: 3px 0;
                margin-bottom: 2px;
                text-align: center;
                font-size: 12px;
                border-radius: 3px;
                color: #3a3092;
                background-color: #e6e6e6;

                &:hover {
                  color: #ffffff;
                  background-color: #e51f76;
                }
              }
            }
          }

          &.me {
            flex-direction: row-reverse;
            margin-right: 10px;

            .item-img {
              margin-right: 0;
              margin-left: 10px;
            }

            .dropdown-menu {
              left: auto !important;
              @media (max-width: 767px) {
                left: 0 !important;
              }
            }

            .msg-text {
              background-color: #3f9de7;
              color: #ffffff;
            }

            .date-time {
              text-align: right;
            }
          }
        }
      }

      .footer {
        position: absolute;
        bottom: 0;
        left: 0;
        width: 100%;

        .footer-top {
          font-size: 12px;
          color: #818181;
          margin-bottom: 12px;
        }

        .footer-bottom {
          .left {
            width: 100%;
            display: flex;
            justify-content: space-between;

            .message-box {
              position: relative;
              width: 100%;

              input {
                height: 36px;
                width: 100%;
                border: 0;
                padding: 7px 54px 7px 40px;
                border-radius: 18px;
                background-color: #eceaf5;
                resize: none;
                //v-on:keyup.enter="sendMsg($event)"
                @media (max-width: 767px) {
                  padding-left: 32px;
                }
              }

              textarea {
                width: 100%;
                border: 0;
                padding: 7px 54px 7px 40px;
                border-radius: 18px;
                background-color: #eceaf5;
                resize: none;
                max-height: 70px;
                overflow-y: hidden;
                @media (max-width: 767px) {
                  padding-left: 32px;
                }
              }

              .btn-emoji, .btn-multimedia, .btn-files {
                position: absolute;
                top: -3px;
                right: 16px;
                margin-left: 10px;
                @media (max-width: 767px) {
                  right: 6px;
                }
              }

              .btn-emoji {
                right: auto;
                left: -8px;
                top: 3px;
              }

              .btn-multimedia {
                right: 45px;
                @media (max-width: 767px) {
                  right: 30px;
                }
              }

              svg {
                .cls-1 {
                  fill: #fff;
                }

                .cls-2 {
                  fill: #8983be;
                }

                .files {
                  fill: #6159a8;
                }

                .multimedia {
                  fill: #6159a8;
                }
              }
            }

            button {
              display: flex;
              justify-content: space-between;
              font-size: 15px;
              height: 36px;
              line-height: 28px;
              @media (max-width: 767px) {
                min-width: auto;
              }

              svg {
                width: 18px;
                float: left;
                margin-top: 9px;
                margin-left: 6px;

                .cls-1 {
                  fill: #fff;
                }

                .cls-2 {
                  fill: #8983be;
                }

                @media (max-width: 767px) {
                  display: none;
                }
              }
            }
          }
        }
      }
    }
  }
}

.rotate-90 {
  transform: rotate(90deg);
}

.br-50 {
  border-radius: 50%;
}

.bg-c9:hover {
  background: #c9c9c9;
}
.microphone {
  width: 15px;
  height: 15px;
}
.chat-item:hover {
  background: #efefef;
  border-radius: 20px;
}

.chat-online {
  color: #34ce57
}

.chat-offline {
  color: #e4606d
}

.chat-message-left,
.chat-message-right {
  display: flex;
  flex-shrink: 0
}

.chat-message-left {
  margin-right: auto
}

.chat-message-right {
  flex-direction: row-reverse;
  margin-left: auto
}
.msg-right-created-at {
  bottom: -4px;
  right: 6px;
}
.msg-left-created-at {
  bottom: -4px;
  left: 6px;
}
.flex-grow-0 {
  flex-grow: 0!important;
}
.border-top {
  border-top: 1px solid #dee2e6!important;
}
.bg-me {
  background-color: #3f9de7;
}
.br-10 {
  border-radius: 10px;
}
.empty-height {
  display: none;
  @media (min-width: 992px) {
    display: flex;
    height: calc(100vh - 700px);
  }
  @media (min-width: 1200px) {
    height: calc(100vh - 200px);
  }
}
.hints-component {
  display: none;
  @media (min-width: 992px) {
    display: block;
  }
}

.chat-messages::-webkit-scrollbar {
  //display: none;
  width: 4px;
}
.conv-user {
  top: -20px;
  right: 10px;
  font-size: 12px;
  font-weight: bold;
}
.conv-left-user {
  top: -20px;
  font-size: 12px;
  font-weight: bold;
}
.conv-mb {
  margin-bottom: 30px;
}
// css custom scrollbar
/* width */
::-webkit-scrollbar {
  width: 10px;
}

/* Track */
::-webkit-scrollbar-track {
  // box-shadow: inset 0 0 5px grey;
  background-color: #e6e6e6;
  border-radius: 10px;
}

/* Handle */
::-webkit-scrollbar-thumb {
  background: #d9d6e9;
  border-radius: 10px;
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
  background: #bcb5de;
}
.btn-short-back {
  top: -20px;
  left: -10px;
  padding: 4px;
  @media (min-width: 576px) {
    display: none;
  }
}
.chat-item-wrapper {
  //height: 500px;
  height: calc(100vh - 250px);
  @media (min-width: 410px) {
    height: calc(100vh - 250px);
  }
  @media (min-width: 576px) {
    height: calc(100vh - 250px);
  }
  @media (min-width: 768px) {
    height: calc(100vh - 250px);
  }
  @media (min-width: 992px) {
    height: calc(100vh - 290px);
  }
  @media (min-width: 1200px) {
    height: calc(100vh - 290px);
  }
  @media (min-width: 1920px) {
    height: calc(100vh - 290px);
  }
}
.chat-area {
  //min-height: 600px;
  min-height: calc(100vh - 260px);
  @media (min-width: 410px) {
    min-height: calc(100vh - 260px);
  }
  @media (min-width: 576px) {
    min-height: calc(100vh - 260px);
  }
  @media (min-width: 768px) {
    min-height: calc(100vh - 260px);
  }
  @media (min-width: 992px) {
    min-height: calc(100vh - 280px);
  }
  @media (min-width: 1200px) {
    min-height: calc(100vh - 280px);
  }
  @media (min-width: 1920px) {
    min-height: calc(100vh - 280px);
  }
}
.chat-messages {
  display: flex;
  flex-direction: column;
  overflow-y: auto;
  //max-height: 540px;
  max-height: calc(100vh - 370px);
  @media (min-width: 410px) {
    max-height: calc(100vh - 350px);
  }
  @media (min-width: 576px) {
    max-height: calc(100vh - 350px);
  }
  @media (min-width: 768px) {
    max-height: calc(100vh - 350px);
  }
  @media (min-width: 992px) {
    max-height: calc(100vh - 365px);
  }
  @media (min-width: 1200px) {
    max-height: calc(100vh - 365px);
  }
  @media (min-width: 1920px) {
    max-height: calc(100vh - 365px);
  }
}
// end css for chat
</style>