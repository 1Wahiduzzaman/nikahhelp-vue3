<template>
    <div class="container-fluid main-container">
      <Loader v-if="isLoading" :isLoading="isLoading" />
      <div class="row" v-else>
        <TeamOffRedirection v-if="redirection" />
        <div class="col-12">
          <div class="chat-wrapper">
            <div class="chat-left"
                :class="{'chat-hide': conversationTitle}">
              <div class="chat-category mb-3">
                <nav>
                  <div class="nav nav-tabs" id="nav-tab" role="tablist">
                    <!-- ------------ to remove team chat tab ------------ -->
                    <!-- <a class="nav-link position-relative w-50"
                      :class="{'active': chatTab == 'Team'}">
                      <span class="online-icon position-absolute" v-if="newMessage || unseenInGroupMsg"></span>
                      <div class="category-item w-100"
                        @click="setChatTab('Team')"
                      >
                        <a>
                          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 42.02 31.17" style="margin-top:3px">
                            <g id="Layer_2" data-name="Layer 2">
                              <g id="middle">
                                <path class="cls-1"
                                    d="M9.37,20.31A13.43,13.43,0,0,0,6.8,24.07,14.3,14.3,0,0,0,5.64,28c0,.34-.07.68-.09,1s0,.67,0,1v1.08H2A2,2,0,0,1,.59,30.5,2,2,0,0,1,0,29.09,12.63,12.63,0,0,1,6,18.34,9.73,9.73,0,0,0,9.37,20.31Zm4.13-3.68a8.66,8.66,0,0,1-1.61-1.7,9.2,9.2,0,0,1-1.74-5.72A8.93,8.93,0,0,1,11,5.64a9.08,9.08,0,0,1,1.72-2.52A8.06,8.06,0,0,0,7.47,17.38a8.16,8.16,0,0,0,2.93,1.54c.28-.28.56-.52.84-.75l.34-.28c.3-.23.58-.44.86-.63a1.43,1.43,0,0,1,1.27-.16l.93.34A8.39,8.39,0,0,1,13.5,16.63Zm19.15,3.68a13.64,13.64,0,0,1,2.57,3.76A14.3,14.3,0,0,1,36.38,28c0,.34.07.68.09,1s0,.67,0,1v1.08H40a2,2,0,0,0,1.42-.59A2,2,0,0,0,42,29.09a12.63,12.63,0,0,0-6-10.75A9.73,9.73,0,0,1,32.65,20.31Zm-5.27-2.87.93-.34a1.43,1.43,0,0,1,1.27.16c.28.19.56.4.86.63l.34.28c.28.23.56.47.84.75a8.06,8.06,0,0,0-2.33-15.8A9.08,9.08,0,0,1,31,5.64a8.93,8.93,0,0,1,.86,3.57,9.2,9.2,0,0,1-1.74,5.72,8.66,8.66,0,0,1-1.61,1.7A8.39,8.39,0,0,1,27.38,17.44Zm2-11.93A9.23,9.23,0,0,0,20.92,0,9.23,9.23,0,0,0,11.7,9.22a9.14,9.14,0,0,0,1.94,5.66,8.6,8.6,0,0,0,1.65,1.65,9.21,9.21,0,0,0,3,1.53,9.21,9.21,0,0,0,8.28-1.53A9,9,0,0,0,28.1,15a9.23,9.23,0,0,0,1.26-9.5Zm-.45,13.11h0a4.06,4.06,0,0,0-.36-.26,1.1,1.1,0,0,0-.14-.09A16.08,16.08,0,0,1,25,19.8a17.09,17.09,0,0,1-4.08.58c-1.2.11-3.87-.67-5.72-1.28l-.48-.16-1.5-.52c-.26.17-.51.36-.75.55l-.32.27A14.67,14.67,0,0,0,11,20.31a13.86,13.86,0,0,0-2.57,3.76A14.28,14.28,0,0,0,7.26,28c0,.34-.07.68-.1,1s0,.67,0,1v1.17L34.7,31v-1.2a13.67,13.67,0,0,0-1.77-6.73,14.08,14.08,0,0,0-3.38-4Zm-.37-.26a4.06,4.06,0,0,1,.36.26m-.36-.26a4.06,4.06,0,0,1,.36.26"/>
                                </g>
                              </g>
                            </svg>
                            <p class="category-name">My Team</p>
                        </a>
                      </div>
                    </a> -->
                    <!-- ------------------------------------------------- -->
                    <a class="nav-link position-relative w-50" :class="{'active': chatTab == 'Connected'}">
                      <span class="online-icon position-absolute" v-if="notify || unseenInConnectedMsg"></span>
                      <div class="category-item w-100" @click="setChatTab('Connected')">
                        <a>
                          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 42.52 39.16" style="margin-top:3px">
                            <g id="Layer_2" data-name="Layer 2">
                              <g id="middle">
                                <path class="cls-1"
                                      d="M24.46,14.64c1.43,1.43,1.62,4.38-3.42,7-5-2.6-4.85-5.55-3.42-7S21,14.17,21,15.3C21,14.17,23,13.2,24.46,14.64ZM8.59,25.1a6.48,6.48,0,1,0-6.48-6.48A6.48,6.48,0,0,0,8.59,25.1Zm1.29,0H7.3A7.37,7.37,0,0,0,0,31.59a.84.84,0,0,0,.85.91H16.32a.84.84,0,0,0,.85-.91A7.37,7.37,0,0,0,9.88,25.15Zm24-7.43a6.48,6.48,0,1,0-6.48-6.48A6.48,6.48,0,0,0,33.93,17.72Zm1.29.05H32.64a7.38,7.38,0,0,0-7.3,6.44.85.85,0,0,0,.86.91H41.66a.84.84,0,0,0,.85-.91A7.37,7.37,0,0,0,35.22,17.77ZM7,11a1.53,1.53,0,0,0,.68.23,1.48,1.48,0,0,0,1.38-.65,12.12,12.12,0,0,1,11-4.86l-.92,2.71,8.72-1.76L22,0,21,2.84A15.07,15.07,0,0,0,6.59,8.94,1.49,1.49,0,0,0,7,11ZM37,26a1.49,1.49,0,0,0-2,.73A12.15,12.15,0,0,1,25,33.22l.48-2.83-8.34,3.1L24,39.16l.51-3h.63a15.08,15.08,0,0,0,12.68-8.25A1.49,1.49,0,0,0,37,26Z"/>
                              </g>
                            </g>
                          </svg>
                          <p class="category-name">Connected team</p>
                        </a>
                      </div>
                    </a>
                    <!-- <a class="nav-link mt-2" :class="{'active': chatTab == 'Request'}">
                      <a-dropdown>
                        <a class="ant-dropdown-link position-relative" @click="e => e.preventDefault()">
                          <a-icon type="more" class="fs-30 font-weight-bolder br-50 bg-c9 color-primary"/>
                          <div class="position-absolute bg-danger chat-request-chip" v-if="privateRequested.length > 0">{{ privateRequested.length }}</div>
                        </a>
                        <a-menu slot="overlay" class="text-center px-2">
                          <a-menu-item class="border-bottom">
                            <a-tooltip title="Coming soon">
                              <a class="fs-12 color-primary">Chat history</a>
                            </a-tooltip>
                          </a-menu-item>
                          <a-menu-item class="border-bottom">
                            <a-tooltip title="Coming soon">
                              <a class="fs-12 color-primary">Archived chat</a>
                            </a-tooltip>
                          </a-menu-item>
                        </a-menu>
                      </a-dropdown>
                    </a> -->
                  </div>
                </nav>
              </div>
              <div class="chat-item-wrapper">
                <div class="tab-content">
                  <!-- ------- to remove team chat tab -------- -->
                  <!-- <div v-if="chatTab == 'Team'" class="tab-pane fade" :class="{'show active': chatTab == 'Team'}">
                    <div class="chat-item"
                      v-for="item in computedTeamChat"
                      :key="item.team_id"
                      :class="{'selected-chat': chatheadopen == item}"
                      @click="getIndividualChat(item, item); newMessage=false"
                    >
                      <ChatListItem
                          :item="item"
                          :status="'team'"
                          :lastMsg="lastTeamMsg"
                          :online_users="online_users"
                          :teamMembers="teamMembers"
                          :newMessages="newMessage"
                          :key="item.id"
                          action
                          class="w-full pr-3 cursor-pointer"
                      />
                    </div>
                  </div> -->
                  <!-- ---------------------------------------- -->
                  <div v-if="chatTab == 'Connected'" class="tab-pane fade" :class="{'show active': chatTab == 'Connected'}">
                    <div class="chat-item"
                         v-for="item in computedConnectedTeam"
                         :key="item.team_id"
                         :class="{'selected-chat': chatheadopen == item}"
                         @click="getConnectedTeamChatHistory(item)"
                    >
                      <ConnectedTeamChat
                          :item="item"
                          :status="'connected'"
                          :online_users="online_users"
                          :teamMembers="teamMembers"
                          :activeTeam="activeTeam"
                          action
                          @socketNotification="socketNotification"
                          :online="notify"
                          class="w-full pr-3 cursor-pointer"
                          :key="item.id"
                      />
                      <!-- {{ item }}
                      {{ online_users }}
                      {{ teamMembers }}
                      {{ activeTeam }}
                      {{ notify }} -->
                    </div>
                  </div>
<!--                  <div v-if="chatTab == 'Private'" class="tab-pane fade" :class="{'show active': chatTab == 'Private'}">-->
<!--                    <div-->
<!--                        class="chat-item"-->
<!--                        v-for="item in chatHistory"-->
<!--                        :key="item.team_id"-->
<!--                        :class="{'selected-chat': chatheadopen == item}"-->
<!--                        @click="item.label == 'Connected Team' ? getConnectedTeamChatHistory(item) : getIndividualChat(item, item)"-->
<!--                    >-->
<!--                      <ChatListItem-->
<!--                          :item="item"-->
<!--                          :status="'recent'"-->
<!--                          :online_users="online_users"-->
<!--                          :teamMembers="teamMembers"-->
<!--                          action-->
<!--                          class="w-full pr-3 cursor-pointer"-->
<!--                      />-->
<!--                    </div>-->
<!--                  </div>-->
                </div>
              </div>
            </div>
            <div class="chat-right position-relative" :class="{'chat-hide': !conversationTitle}" v-if="chatheadopen">
              <div class="header clearfix">
                <div class="left">
                  <div class="top">
                    <h4 class="cursor-pointer btn-mobile-back" @click="backToTabList()"><ArrowLeftOutlined /></h4>
                    <div class="item-img conv-head-logo">
                      <img :src="chatheadopen.logo ? chatheadopen.logo + `?token=${token}` : getImage()" alt="info image">
                    </div>
                    <div class="chat-info">
                      <div class="chat-group bg-primary">{{ chatheadopen.label }}</div>
                      <div class="chat-name">
                        <a-tooltip bottom :title="chatheadopen.label">
                          {{ conversationTitle }}
                        </a-tooltip>
                      </div>
                      <a-tooltip :title="chatheadopen ? getTeamOnlineUsersNames : ''">
                        <div class="last-chat">Active now {{ getTeamOnlineUsers() > 0 ? getTeamOnlineUsers() : '' }}</div>
                      </a-tooltip>
                    </div>
                  </div>
                </div>
                <div class="right">
                  <a-dropdown>
                    <a class="ant-dropdown-link py-2" @click="e => e.preventDefault()">
                      <MoreOutlined class="fs-30 font-weight-bolder rotate-90 br-50 bg-c9 color-primary"/>
                    </a>
                    <a-menu onContent="overlay" class="text-center px-2">
                      <a-menu-item class="border-bottom">
                        <a-tooltip title="Coming soon">
                          <a class="fs-12 color-primary">Mute this chat</a>
                        </a-tooltip>
                      </a-menu-item>
                      <a-menu-item class="border-bottom">
                        <a-tooltip title="Coming soon">
                          <a class="fs-12 color-primary">Mark as read</a>
                        </a-tooltip>
                      </a-menu-item>
                      <a-menu-item class="border-bottom">
                        <a-tooltip title="Coming soon">
                          <a class="fs-12 color-primary">Add to chat history</a>
                        </a-tooltip>
                      </a-menu-item>
                      <a-menu-item class="border-bottom">
                        <a-tooltip title="Coming soon">
                          <a class="fs-12 color-primary">Archive chat</a>
                        </a-tooltip>
                      </a-menu-item>
                      <a-menu-item>
                        <a-tooltip title="Coming soon">
                          <a class="text-danger fs-12">Delete chat</a>
                        </a-tooltip>
                      </a-menu-item>
                    </a-menu>
                  </a-dropdown>
                </div>
              </div>
              <div class="chat-area">
                <div class="position-relative">
                  <div class="chat-messages py-4 pr-1" ref="chatMessages"   id="chat-messages">
                    <div v-for="(item, cIndex) in chatFilter" 
                          :key="item.id" 
                          class="position-relative"
                          :id="chatFilter.length === cIndex + 1 ? 'messagesid' : ''">
                      <div
                           class="chat-message-right pb-4 position-relative"
                           :class="{'conv-mb': chats.length !== cIndex + 1}"
                           v-if="(parseInt(item.senderId) == parseInt(getAuthUserId)) || (parseInt(item.sender) == parseInt(getAuthUserId))" >
                        <div class="text-right">
                          <img :src="getAuthUser && getAuthUser.per_main_image_url ? getAuthUser.per_main_image_url + `?token=${token}` : getImage()" class="rounded-circle mr-1" alt="" width="40" height="40">
                        </div>
                        <div class="flex-shrink-1 py-2 px-3 mr-3 bg-me text-break text-white br-10" >
                          {{ item.body}}
                        </div>
                        <div class="text-muted small text-nowrap px-3 mt-2 position-absolute msg-right-created-at">
                          {{ messageCreatedAt(item.created_at) }}<span>, Me</span>
                        </div>
                      </div>

                      <div
                           class="chat-message-left pb-4 position-relative"
                           :class="{'conv-mb': chats.length !== cIndex + 1}" v-else>
                        <div class="text-left">
                          <img :src="getConversationUserImage(item.sender.id)" class="rounded-circle mr-1" width="40" height="40">
                        </div>
                        <div class="flex-shrink-1 bg-light py-2 px-3 ml-3 text-break br-10">
                          {{ item.body }}
                        </div>
                        <div class="text-muted small text-nowrap px-3 mt-2 position-absolute msg-left-created-at">
                          {{ messageCreatedAt(item.created_at) }}<span>, {{ item.sender ? item.sender.full_name : '' }}</span>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>

                <div class="footer">
                   <div class="footer-top"><strong>{{ chatheadopen.typer_name }}</strong> {{ chatheadopen.typing_text }}</div>
                  <div class="footer-bottom">
                    <form action="#" @submit.prevent>
                      <div class="left flex justify-content-end align-items-end">
                        <div class="message-box">
                          <a-tooltip>
                            <!-- <template slot="title">
                              Coming soon
                            </template> -->
                            <button class="btn-emoji px-2" title="Coming soon" @click="openEmoji=!openEmoji">&#128578;</button>
                            <div class="wrapper">

                              <!-- <emoji-picker @emoji="append" :search="search">
                                <button
                                  class="emoji-invoker"
                                  slot="emoji-invoker"
                                  slot-scope="{ events: { click: clickEvent } }"
                                  @click.stop="clickEvent"
                                >
                                   <svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 fill-current text-grey">
                                      <path d="M0 0h24v24H0z" fill="none"/>
                                      <path d="M11.99 2C6.47 2 2 6.48 2 12s4.47 10 9.99 10C17.52 22 22 17.52 22 12S17.52 2 11.99 2zM12 20c-4.42 0-8-3.58-8-8s3.58-8 8-8 8 3.58 8 8-3.58 8-8 8zm3.5-9c.83 0 1.5-.67 1.5-1.5S16.33 8 15.5 8 14 8.67 14 9.5s.67 1.5 1.5 1.5zm-7 0c.83 0 1.5-.67 1.5-1.5S9.33 8 8.5 8 7 8.67 7 9.5 7.67 11 8.5 11zm3.5 6.5c2.33 0 4.31-1.46 5.11-3.5H6.89c.8 2.04 2.78 3.5 5.11 3.5z"/>
                                  </svg>
                                </button>
                                <div slot="emoji-picker" slot-scope="{ emojis, insert }">
                                  <div class="emoji-picker" :style="{ top: -310 + 'px', left: 32 + 'px' }">
                                    <div class="emoji-picker__search">
                                      <input class="p-3" type="text" v-model="search" v-focus>
                                    </div>
                                    <div>
                                      <div v-for="(emojiGroup, category) in emojis" :key="category">
                                        <h5>{{ category }}</h5>
                                        <div class="emojis">
                                          <span
                                            v-for="(emoji, emojiName) in emojiGroup"
                                            :key="emojiName"
                                            @click="insert(emoji)"
                                            :title="emojiName"
                                          >{{ emoji }}</span>
                                        </div>
                                      </div>
                                    </div>
                                  </div>
                                </div>
                              </emoji-picker> -->
                              <!-- <emoji-picker :native="true" @select="append" /> -->
                              <emoji-picker
                                v-show="openEmoji"
                                picker-type="text-area"
                                @select="append"
                              />
                            </div>
                          </a-tooltip>
                          <textarea class="regular-input" name="message" id=""  rows="4" placeholder="Type message..."
                                    v-model="msg_text" @keydown.enter.exact.prevent="sendMsg($event)" @keyup="notifyKeyboardStatus" @click="openEmoji=false"></textarea>
                          <div class="position-absolute msgbox-right">
                            <div class="flex">
                              <a-tooltip>
                                <template #title>
                                  Coming soon
                                </template>
                                <button v-if="false"><img src="../../assets/icon/microphone.png" alt="icon" class="mr-2 microphone" /></button>
                              </a-tooltip>
                              <!-- <a-tooltip>
                                <template #title>
                                  Coming soon
                                </template>
                                <button><a-icon type="file-image" class="color-primary" /></button>
                              </a-tooltip> -->
                            </div>
                          </div>
                        </div>
                        <button class="btn btn-primary btn-submit js-msg-send" @click="sendMsg">
                          <div class="flex w-100 h-100  justify-content-center align-items-center">
                            <div class="flex">
                              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 25.68 18.77">
                                <g id="Layer_2" data-name="Layer 2">
                                  <g id="middle" fill="white">
                                    <polygon class="cls-1" points="8.6 12.29 7.06 13.34 6.86 11.05 8.6 12.29"/>
                                    <polygon class="cls-1" points="0 6.23 25.68 0 17.75 18.77 0 6.23"/>
                                    <polygon class="cls-2" points="6.86 11.05 24.95 0.57 8.6 12.29 6.86 11.05"/>
                                  </g>
                                </g>
                              </svg>
                            </div>
                            <div class="send-text">
                              Send
                            </div>
                          </div>
                        </button>
                      </div>
                    </form>
                  </div>
                </div>
              </div>
            </div>

            <div class="chat-right" v-else>
              <div class="flex justify-content-center align-items-center empty-height">
                <h4 class="fs-20 flex flex-column align-items-center justify-content-center">Select a conversation & start the chat</h4>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
// import ChatListItem from '@/components/notification/ChatListItem.vue';
import ApiService from '@/services/api.service';
import {map, pick} from 'lodash';
import {format} from 'timeago.js'
import JwtService from "@/services/jwt.service";
// import {openModalRoute} from "@/plugins/modal/modal.mixin";
import ConnectedTeamChat from "../../components/chat/ConnectedTeamChat.vue";
// import PrivateRequestChat from "../../components/chat/PrivateRequestChat.vue";
import Notification from "@/common/notification.js";
import TeamOffRedirection from "../../components/redirection/TeamOffRedirection.vue";
import 'vue3-emoji-picker/css';
import EmojiPicker from 'vue3-emoji-picker'
import { chatEventBus } from '@/eventBuses/chatEventBus';

import {ArrowLeftOutlined, MoreOutlined} from '@ant-design/icons-vue';

import InfoImg from '@/assets/info-img.png';

const messageKeys = ['id', 'user_id', 'chat_id', 'team_id', 'from_team_id', 'to_team_id', 'private_receiver_id', 'private_team_chat_id', 'body', 'seen', 'created_at'];

export default {
  name: 'ChatWindow',

  // vue socket is not working in vue 3 so we have commented out the sokcets
  // sockets: {
  //   connect() {
  //   },

  //   ping(data) {
  //   },

  //   receive_message(data) {
  //     console.log(data, 'data from socket receive_message');
  //     const recieveMessage = {
  //       sender: data.senderInfo,
  //       created_at: data.created_at,
  //       updated_at: data.updated_at,
  //       body: data.body,
  //     };
  //     if(data.label == 'Connected') {
  //       this.notify = data;
  //     }
  //     if (this.chatTab === 'Connected') {
  //       if(!data.team_id) {
  //         recieveMessage.team_connection_id = data.team_connection_id;
  //         recieveMessage.id = data.msg_id;
  //         recieveMessage.team_chat_id = data.team_chat_id;
  //         if(this.connectedTeam) {
  //           this.connectedTeam = this.connectedTeam.map(item => {
  //             if(item.id == data.team_connection_id) {
  //               item.message.id = data.msg_id;
  //               item.message.body = data.body;
  //               item.message.team_chat_id = data.team_chat_id;
  //             }
  //             return item;
  //           });
  //         }
  //         this.connectedTeamChats = [...this.connectedTeamChats, recieveMessage];
  //         this.notify = data;
  //       }
  //     } else {
  //       if(data.team_id) {
  //         recieveMessage.id = data.msg_id;
  //         if(this.teamChat) {
  //           this.teamChat = this.teamChat.map(item => {
  //             item.message.id = data.msg_id;
  //             item.message.body = data.body;
  //             item.message.team_id = data.team_id;
  //             return item;
  //           });
  //         }
  //         this.chats = [...this.chats, recieveMessage];
  //         this.newMessage = true;
  //       }
  //     }
  //   },
  // },

  data() {
    return {
      redirection: false,
      conversationTitle: '',
      type: 'team chat',
      msg_text: '',
      teamChat: [],
      chatHistory: [],
      chats: [],
      conv_search_key: null,
      activeTeam: null,
      teamMembers: [],
      connectedTeam: [],
      privateRequests: [],
      online_users: [],
      one_to_one_user: null,
      inConnectedChat: false,
      chat_type: null,
      chat_id: null,
      // chatTab: 'Team',   // to remove team chat tab
      chatTab: 'Connected',
      other_mate_id: null,
      private_chat: {
        to_team_id: null,
        receiver: null
      },
      chatheadopen: null,
      isLoading: false,
      fromChatItem: null,
      active_team_id: null,
      chatListedImage: [],
      connectedTeamChats: [],
	    notify: false,
      newMessage: false,
      token: "",
      search: '',
      openEmoji: false,
      InfoImg,
    }
  },

  components: {
    TeamOffRedirection,
    // PrivateRequestChat,
    ConnectedTeamChat,
    // ChatListItem,
    EmojiPicker,

    ArrowLeftOutlined,
    MoreOutlined
  },

  watch: {
    chatFilter: 'scrollBottom',

    chatTab() {
      this.chatheadopen = null;
      this.loadPageData();
    }
  },

  computed: {
    privateRequested() {
      if(this.teamMembers && this.teamMembers.length > 0) {
        return this.privateRequests.filter(item => item.is_friend == 0 && this.teamMembers.includes(item.receiver.toString()));
      }
      return [];
    },
    getTeamOnlineUsersNames() {
      let onlineUsersNames = [];
      if(this.chatTab == 'Team') {
        let onlineUsers = this.online_users.filter(item => this.teamMembers.includes(item));
        if(onlineUsers.length > 0) {
          onlineUsersNames = this.chatheadopen?.team_members?.filter(item => onlineUsers.includes(item.user_id.toString())).map(item => {
            return item.user.full_name;
          });
        }
        return onlineUsersNames.join(', ');
      } else if(this.chatTab == 'Connected') {
        let online_members = [];
        this.chatheadopen?.to_team?.team_members?.forEach(item => {
          if(this.online_users.includes((item.user_id).toString())) {
            online_members.push(item.user_id);
          }
        });
        this.chatheadopen?.from_team?.team_members?.forEach(item => {
          if(this.online_users.includes((item.user_id).toString())) {
            online_members.push(item.user_id);
          }
        });

        if(online_members.length > 0) {
          onlineUsersNames = this.chatheadopen?.to_team?.team_members?.filter(item => online_members.includes(item.user_id)).map(item => {
            return item.user.full_name;
          });
          onlineUsersNames = onlineUsersNames.concat(this.chatheadopen?.from_team?.team_members?.filter(item => online_members.includes(item.user_id)).map(item => {
            return item.user.full_name;
          }));
        }

        return onlineUsersNames.join(', ');
      }

      return onlineUsersNames.join(',');
    },
    totalUnreadCount: function () {
      return this.$store.state.chat.unread_records.length;
    },

    teamUnreadCount: function () {
      var count = 0;
      for (var i = 0; i < this.$store.state.chat.unread_records.length; i++) {
        if (this.$store.state.chat.unread_records[i].conv_type == 'team chat') {
          count = count + 1;
        }
      }
      return count;
    },

    connectedUnreadCount: function () {
      var count = 0;
      for (var i = 0; i < this.$store.state.chat.unread_records.length; i++) {
        if (this.$store.state.chat.unread_records[i].conv_type == 'connected') {
          count = count + 1;
        }
      }
      return count;
    },

    connectedConversations: function () {
      var connectedConversations = [];
      if (this.conv_search_key == null || this.conv_search_key == "") {
        for (let i = 0; i < this.$store.state.chat.conversations.length; i++) {
          if (this.$store.state.chat.conversations[i].type == 'connected') {
            connectedConversations.push(this.$store.state.chat.conversations[i]);
          }
        }
        return connectedConversations;
      } else {
        var lKey = this.conv_search_key.toLowerCase();
        for (let i = 0; i < this.$store.state.chat.conversations.length; i++) {
          var lTitle = this.$store.state.chat.conversations[i].title.toLowerCase();
          if (this.$store.state.chat.conversations[i].type == 'connected') {

            if (lTitle.includes(lKey)) {
              connectedConversations.push(this.$store.state.chat.conversations[i]);
            }
          }
        }
        return connectedConversations;
      }
    },

    getAuthUserId() {
      let loggedUser = JSON.parse(localStorage.getItem('user'));
      if (loggedUser) {
        return loggedUser.id;
      }
      return null;
    },
    getAuthUser() {
      return JSON.parse(localStorage.getItem('user'));
    },
    getChatType() {
      if (this.inConnectedChat) {
        return 'Connected Group';
      } else {
        return this.one_to_one_user ? 'Private' : this.conversationTitle + ' Group'
      }
    },

    chatFilter() {
      return this.chatTab === 'Connected' ? this.connectedTeamChats.filter(item => {
        if(item.team_connection_id) {
          return item.team_connection_id == this.chatheadopen?.id;
        } else {
          return item
        }
      }) : this.chats;
    },

    lastTeamMsg() {
      return this.chats[this.chats.length - 1];
    },
    computedConnectedTeam() {
      return this.connectedTeam;
    },
    computedTeamChat() {
      return this.teamChat;
    },
    unseenInGroupMsg() {
      if(this.$store.state.chat.chats.length > 0) {
        let index = this.$store.state.chat.chats.findIndex(item => item.label = 'Group chat');
        if(index != -1) {
          return this.$store.state.chat.chats[index].message.id != this.$store.state.chat.chats[index].last_seen_msg_id;
        } else {
          return false;
        }
      } else {
        return false;
      }
    },
    unseenInConnectedMsg() {
      for(let i = 1; i < this.$store.state.chat.chats.length; i++) {
        if(this.$store.state.chat.chats[i].label != 'Group chat' && this.$store.state.chat.chats[i].message.id != this.$store.state.chat.chats[i].last_seen_msg_id) {
          return true;
        }
      }
      return false;
    },
    routerParams() {
      if(this.$route.params.connection_id) {
        return true;
      } else {
        return false;
      }
    },
    isWebSocketReady() {
      return this.$webSocket.readyState === 1;
    }
  },

  // directives: {
  //     scrollBottom: {
  //       inserted(el) {

  //       }
  //     }
  // },

  created() {
    this.getActiveTeamId();

    // --------- to remove team chat tab ---------
    // console.log("this.$route.query.connection_id", this.$route.query.connection_id)
    // if(this.routerParams) {
    //   this.setChatTab('Connected');
    // }else {
    //   this.setChatTab('Team');
    // };
    // -------------------------------------------
    this.getToken();
  },

  mounted() {
    chatEventBus.on('receive_message', this.handleReceiveMessage);

    chatEventBus.on('receive_notification', this.handleReceiveNotification);
    
    chatEventBus.on('lis_typing', this.handleLisTyping);
  },
  beforeUnmount() {
    console.log('beforeUnmount');
    chatEventBus.off('receive_message', this.handleReceiveMessage);
    chatEventBus.off('receive_notification', this.handleReceiveNotification);
    chatEventBus.off('lis_typing', this.handleLisTyping);
  },
  directives: {
    focus: {
      inserted(el) {
        el.focus()
      },
    },
  },

  methods: {
    getToken() {
      this.token = JSON.parse(localStorage.getItem("token"));
    },
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
      this.loadPageData();
    },
    async loadPageData() {
      this.isLoading = true;
      await this.loadTeamChat();
      this.isLoading = false;
      this.loadChatHistory();
      this.loadConnectedGroup();
      this.getPrivateRequests();
    },

    getActiveTeamId() {
      if (!JwtService.getTeamIDAppWide()) {
        this.redirection = true;
      } else {
        this.active_team_id = JwtService.getTeamIDAppWide();
        this.loadPageData();
      }
    },

    setChatTab(type) {
      this.chatTab = type;
    },
    async sendMsg(e) {
      this.openEmoji = false;
      console.log(e);
      if (this.msg_text && this.msg_text.length > 0) {
        if (this.chatheadopen.label == 'Connected Team') {
          await this.sendConnectedTeamMessage();
        } else {
          if(this.chatheadopen && this.chatheadopen.team_private_chat_id) {
            await this.sendPrivateMessage();
          } else {
            await this.sendTeamMessage();
          }
        }
      }
    },
    ifOnline(item) {
      if(item.label === 'Group chat') {
        return this.onlineTeam(item);
      } else if(item.label == 'Team member' || item.label == 'Private chat') {
        return this.onlineUser(item);
      }
      return false;
    },
    getTeamOnlineUsers() {
      if(this.chatTab == 'Team') {
        return this.online_users.filter(item => this.teamMembers.includes(item)).length;
      } else if(this.chatTab == 'Connected') {
        let online_members = [];
        this.chatheadopen?.to_team?.team_members?.forEach(item => {
          if(this.online_users.includes((item.user_id).toString())) {
            online_members.push(item.user_id);
          }
        });
        this.chatheadopen?.from_team?.team_members?.forEach(item => {
          if(this.online_users.includes((item.user_id).toString())) {
            online_members.push(item.user_id);
          }
        });

        return online_members.length;
      }
    },
    onlineTeam(item) {
      let team_members = [];
      if(item && this.teamMembers && this.teamMembers.length > 0) {
        console.log(this.teamMembers)
        team_members = this.teamMembers.map(i => {
          return parseInt(i);
        });
      }
      let online = this.online_users.find(item => team_members.includes(parseInt(item)));
      if(online) {
        return true;
      } else {
        return false;
      }
    },
    onlineUser(item) {
      if(item && item.other_mate_id && this.online_users.includes(parseInt(item.other_mate_id))) {
        return true;
      } else {
        return false;
      }
    },
    // Process team chat response
    processTeamChatResponse(data) {
      // let group = pick(data, ['id', 'name', 'logo']);
      this.activeTeam = data.id;
      let group = data;
      group.message = pick(data.last_group_message, messageKeys);
      group.label = 'Group chat';
      group.state = 'Typing...';
      group.typing_status = 0;
      group.typing_text = '';

      return [group, ...map(data.team_members, item => {
        return {
          label: 'Team member',
          user_id: item.user_id,
          state: 'seen',
          name: item.user?.full_name || 'user name',
          logo: item.user && item.user.candidate_info && item.user.candidate_info.per_main_image_url ? item.user.candidate_info.per_main_image_url + `?token=${this.token}` : this.InfoImg,
          other_mate_id: item.user_id,
          typing_status: 0,
          typing_text: '',
          message: pick(item.last_message, messageKeys)
        }
      })];
    },
    processChatHistoryResponse(data) {
      let loggedUser = JSON.parse(localStorage.getItem('user'));


      let privateChat = map(data.private_chat, item => {
        return {
          label: 'Private chat',
          state: 'seen',
          name: this.getPrivateChatUserName(item)?.full_name,
          logo: this.getPrivateChatLogo(item),
          to_team_id: item.to_team_id,
          from_team_id: item.from_team_id,
          private_receiver_id: item.receiver,
          team_private_chat_id: item.id,
          other_mate_id: item.receiver,
          typing_status: 0,
          typing_text: '',
          is_friend: 1,
          private_receiver_data: loggedUser.id == item.sender ? item.private_receiver_data : item.private_sender_data,
          message: pick(item.last_private_message, messageKeys)
        }
      });


      return [...privateChat];
    },
    getPrivateChatUserName(item) {
      let loggedUser = JSON.parse(localStorage.getItem('user'));
      return loggedUser.id == item.sender ? item.private_receiver_data : item.private_sender_data;
    },
    getPrivateChatLogo(item) {
      let loggedUser = JSON.parse(localStorage.getItem('user'));
      let opositeUser = loggedUser.id == item.sender ? item.private_receiver_data : item.private_sender_data;
      if(opositeUser && opositeUser.candidate_info && opositeUser.candidate_info.per_main_image_url) {
        return opositeUser.candidate_info.per_main_image_url + `?token=${this.token}`;
      }
      return this.InfoImg;
    },
    async loadTeamChat() {
      try {
        let {data} = await ApiService.get(`/v1/team-chat?team_id=${this.active_team_id}`).then(res => res.data);
        let last_seen_data = await ApiService.get('/v1/own-team-last-seen').then(res => res.data.data);

        if (data && data.team_members) {
          this.teamMembers = map(data.team_members, item => {
            return item.user_id.toString();
          });
        }
        this.teamChat = this.processTeamChatResponse(data);
        this.teamChat = this.teamChat.filter(e => e.id !== undefined);
        this.teamChat = this.teamChat.map(item => {
          item.last_seen_msg_id = last_seen_data.last_seen_msg_id;
          return item;
        });
      } catch (e) {
        console.error(e);
      }
    },
    async loadConnectedGroup() {
      try {
        let payload = {
          team_id: 1
        };
        let {data} = await ApiService.post('/v1/connection-list-chat', payload).then(res => res.data);
        let last_seen_data = await ApiService.get('/v1/connected-team-last-seen').then(res => res.data.data);
        this.connectedTeam = data.map(item => {
          item.label = 'Connected Team';
          item.typing_status = 0;
          item.typing_text = '';
          item.logo = this.getConnectedTeamInfo(item) ? this.getConnectedTeamInfo(item).logo : '';
          item.message = item.team_chat && item.team_chat?.last_message ? item.team_chat.last_message : {};
          item.is_friend = item.team_private_chat ? item.team_private_chat.is_friend : 0;

          let lastSeen = last_seen_data.find(i => i.team_chat_id == item.team_chat?.id);
          if(lastSeen) {
            item.last_seen_msg_id = lastSeen.last_seen_msg_id;
          }

          return item;
        });

        if(this.$route.query.connection_id) {
          let queryChat = this.connectedTeam.find(chat => chat.id == this.$route.query.connection_id);
          if(queryChat) {
            this.getConnectedTeamChatHistory(queryChat);
          }

          const query = Object.assign({}, this.$route.query);
          delete query.connection_id;
          this.$router.replace({ query });
        }

      } catch (e) {
        console.error(e);
      }
    },
    getConnectedTeamInfo(item) {
      if(item.from_team_id == this.activeTeam) {
        return item.to_team ? item.to_team : null;
      } else {
        return item.from_team ? item.from_team : null;
      }
    },
    async getPrivateRequests() {
      let {data} = await ApiService.get('/v1/get-all-private-chat-requests').then(res => res.data);
      this.privateRequests = data.map(item => {
        item.label = 'Private chat request';
        item.typing_status = 0;
        item.typing_text = '';
        item.logo = this.getPrivateChatLogo(item);
        return item;
      });
    },
    async loadChatHistory() {
      try {
        let {data} = await ApiService.get('/v1/chat-history').then(res => res.data);
        this.chatHistory = this.processChatHistoryResponse(data);
      } catch (e) {
        console.error(e);
      }
    },
    async loadIndividualChatHistory(payload) {
      try {
        let url = 'individual-chat-history';
        if(payload.from_team_id) {
          url = 'connected-team-chat-history';
        }
        if(this.chatheadopen.team_private_chat_id) {
          url = 'connected-private-chat-history';
          payload.team_private_chat_id = this.chatheadopen.team_private_chat_id;
          payload.team_chat_id = this.chatheadopen.team_private_chat_id;
        }
        let {data} = await ApiService.post(`/v1/${url}`, payload).then(res => res.data);
        if (data && data.message_history) {
          data = data.message_history;
        }
        return data.map(item => {
          item.senderId = item.sender?.id
          return item;
        });
      } catch (e) {
        console.error(e);
      }
    },
    async getConnectedChat(item) {
      try {
        console.log(item)
        this.conversationTitle = item.name;
        this.inConnectedChat = true;
        this.chat_type = 'connected-team';
        let payload = {
          to_team_id: 1
        };
        let {data} = await ApiService.post('/v1/connected-team-chat-history', payload).then(res => res.data);
        this.connectedTeamChats = data.map(item => {
          item.senderId = item.sender?.id
          return item;
        });
      } catch (e) {
        console.error(e);
      }
    },
    async getIndividualChat({message: {chat_id, team_id}, name, user_id, from_team_id, to_team_id, private_receiver_id, private_team_chat_id}, item) {
      if(this.msg_text) {
        this.msg_text = '';
        this.notifyKeyboardStatus();
      }
      const payload = {
        type: chat_id ? 'single' : 'team',
        chat_id,
        team_id,
        user_id,
        from_team_id,
      };

      this.conversationTitle = '';
      this.one_to_one_user = user_id;

      // const isAnyKeyValueFalse = !!Object.keys(payload).find(k => !payload[k]);

      // this.activeTeam = team_id;
      this.conversationTitle = name;
      this.inConnectedChat = false;

      if(this.one_to_one_user) {
        this.chat_type = 'one-to-one';
        this.private_chat = {};
        this.other_mate_id = user_id;
      } else {
        if(chat_id) {
          this.chat_type = 'one-to-one';
          this.chat_id = chat_id;
          this.private_chat = {};
          this.other_mate_id = user_id;
        } else {
          if(private_receiver_id) {
            this.chat_type = 'private';
            this.other_mate_id = private_receiver_id;
            this.private_chat = {
              to_team_id: to_team_id,
              receiver: private_receiver_id,
              private_team_chat_id: private_team_chat_id
            }
            payload.team_chat_id = private_team_chat_id;
            payload.to_team_id = 1;
          } else {
            this.chat_type = 'team';
            this.private_chat = {};
          }
        }
      }
      this.chatheadopen = item;
      this.chatheadopen.message.seen = 1;


      this.processChatConnectedImage();
      this.chats = [];
      this.chats = await this.loadIndividualChatHistory(payload);

      let payload2 = {
        team_id: this.activeTeam,
        last_seen_msg_id: this.chats.length > 0 ? this.chats[this.chats.length - 1]?.id : null
      }

      this.teamChat[0].last_seen_msg_id = payload2.last_seen_msg_id;

      await ApiService.post('/v1/own-team-last-seen', payload2).then(res => res.data).catch(err => console.log(err));
      this.$store.state.chat.chats = this.$store.state.chat.chats.map(item => {
        if(item.label == 'Group chat') {
          item.message.body = this.chats[this.chats.length - 1].body;
          item.message.id = payload2.last_seen_msg_id;
          item.last_seen_msg_id = payload2.last_seen_msg_id;
        }
        return item;
      }) 
    },
    async getConnectedTeamChatHistory(item) {
			this.notify = false;
      this.fromChatItem = 'connected-team';
      this.chat_type = 'Connected-team';
      this.inConnectedChat = true;
      let to_team_id = null;
      if(item.from_team_id == this.activeTeam) {
        this.conversationTitle = item.to_team ? item.to_team.name : 'N/A';
        to_team_id = item.to_team_id;
      } else {
        this.conversationTitle = item.from_team ? item.from_team.name : 'N/A';
        to_team_id = item.from_team_id;
      }
      this.private_chat = {};
      item.label = 'Connected Team';
      this.chatheadopen = item;
      this.chatheadopen.message.seen = 1;


      // update last seen msg id
      let payload = {};
      let new_msg_from_socket = false;   // we are supposing that we have new msg from socket if we are in connected team chat
      for(let i = this.connectedTeamChats.length -1; i >= 0; i--) {
        if(this.connectedTeamChats[i].team_connection_id == this.chatheadopen.id) {
          new_msg_from_socket = true;
          payload = {
            team_chat_id: this.connectedTeamChats[i].team_chat_id,
            last_seen_msg_id: this.connectedTeamChats[i].id,
          }
          break;
        }
      }
      if(!new_msg_from_socket) {
        payload = {
          team_chat_id: this.chatheadopen.message?.team_chat_id,
          last_seen_msg_id: this.chatheadopen.message?.id,
        }
      }
      

      this.connectedTeam = this.connectedTeam.map(item => {
        if(item.id == this.chatheadopen.id) {
          item.last_seen_msg_id = this.chatheadopen.message?.id;
        }
        return item;
      });



      await ApiService.post('/v1/connected-team-last-seen', payload).then(res => res.data).catch(err => console.log(err));
      this.$store.state.chat.chats = this.$store.state.chat.chats.map(item => {
        if(item.message.team_chat_id == payload.team_chat_id) {
          item.message.id = payload.last_seen_msg_id;
          item.last_seen_msg_id = payload.last_seen_msg_id;
        }
        return item;
      })

      this.processChatConnectedImage();


      // get new connected team chat history
	    this.connectedTeamChats = [];
      let url = 'connected-team-chat-history';
      payload = {
        to_team_id: to_team_id
      };
      let {data} = await ApiService.post(`/v1/${url}`, payload).then(res => res.data);
      if (data && data.message_history) {
        data = data.message_history;
      }

      this.connectedTeamChats = data.map(item => {
        item.senderId = item.sender?.id
        return item;
      });

      this.scrollBottom();
    },
    processChatConnectedImage() {
      this.chatListedImage = [];
      if(this.chatheadopen.label == 'Group chat') {
        this.chatheadopen.team_members.forEach(member => {
          let candidateLogo = member && member.user && member.user.candidate_info ? member.user.candidate_info.per_main_image_url + `?token=${this.token}` : '';
					let repPhoto = member && member.user && member.user.representative_info ? member.user.representative_info.per_main_image_url + `?token=${this.token}` : '';

					if (!member.user.candidate_info) {
						this.chatListedImage.push({
							user_id: member.user_id,
							logo: repPhoto
						});

					} else {
            this.chatListedImage.push({
              user_id: member.user_id,
              logo: candidateLogo
            });
          }

        });
      } else if(this.chatheadopen.label == 'Connected Team') {
        this.chatheadopen.from_team.team_members.forEach(member => {
          let candidateLogo = member && member.user && member.user.candidate_info ? member.user.candidate_info.per_main_image_url + `?token=${this.token}` : '';
					let repPhoto = member && member.user && member.user.representative_info ? member.user.representative_info.per_main_image_url + `?token=${this.token}` : '';
          if (!member.user.candidate_info) {
            console.log(repPhoto, 'rep phpoto')
						this.chatListedImage.push({
							user_id: member.user_id,
							logo: repPhoto
						});

					} else {
            this.chatListedImage.push({
              user_id: member.user_id,
              logo: candidateLogo
            });
          }
        });

        this.chatheadopen.to_team.team_members.forEach(member => {
          let candidateLogo = member && member.user && member.user.candidate_info ? member.user.candidate_info.per_main_image_url + `?token=${this.token}` : '';
					let repPhoto = member && member.user && member.user.representative_info ? member.user.representative_info.per_main_image_url + `?token=${this.token}` : '';
          if(!member.user.candidate_info) {
            this.chatListedImage.push({
              user_id: member.user_id,
              logo: repPhoto
            });
          } else {
            this.chatListedImage.push({
              user_id: member.user_id,
              logo: candidateLogo
            });
          }
        });
      } else if(this.chatheadopen.label == 'Private chat') {
        this.chatListedImage.push({
          user_id: this.chatheadopen.other_mate_id,
          logo: this.chatheadopen.logo
        });
      } else if(this.chatheadopen.label == 'Team member') {
        this.chatListedImage.push({
          user_id: this.chatheadopen.other_mate_id,
          logo: this.chatheadopen.logo
        });
      }
    },
    messageCreatedAt(time) {
      return format(time);
    },

    backToTabList() {
      this.conversationTitle = '';
    },

    createConversations() {
      // console.log('create conv clicked');
      // var member_1 = Math.floor(Math.random() * 6) + 1;
      // var member_2 = Math.floor(Math.random() * 7) + 1;
      // var member_3 = Math.floor(Math.random() * 8) + 1;
      // var member_4 = Math.floor(Math.random() * 9) + 1;

      // var members = [8, 9, 10, 11];
      // members = this.unique(members);

      // var conv_title = 'Group:'+members[0].toString()+","+members[1].toString()+','+members[2].toString();
      // var conv_title = 'test conv';

      // var total = member_1+member_2+member_3;
      // var conv_type = '';
      // if(total % 2 == 0) {
      //     conv_type = 'team chat';
      // }
      // else{
      //   conv_type = 'connected';
      // }

      // var conv_type = 'connected';

      // var newConv = {
      //   title: conv_title,
      //   type: conv_type,
      //   members: members,
      //   last_msg: ''
      // }
    },
    selectConversation(conv_id) {
      // this.current_conversation = conv_id;
      this.$store.dispatch('setCurrentConversation', conv_id);

      var user_id = this.$store.state.user.user.id;
      var payload = {
        user_id: user_id,
        conv_id: conv_id
      }

      this.$store.dispatch('clearUnreadMsg', payload);
    },

    async sendTeamMessage() {
      let url = '';
      let loggedUser = JSON.parse(localStorage.getItem('user'));
      let payload = {
        support: false,
        team_id: this.activeTeam.toString(),
        body: this.msg_text,
        message: this.msg_text,
        created_at: new Date(),
        senderId: loggedUser.id.toString(),
        senderInfo: loggedUser,
        logo: this.chatheadopen.logo
      }

      if (this.one_to_one_user || this.chat_id) {
        payload.receiver = this.one_to_one_user.toString()
        payload.target_opened_chat = loggedUser.id;
        payload.target_opened_chat_type = 'one-to-one';
        payload.target_opened_chat_id = this.chat_id;
        payload.conv_title = this.conversationTitle;
        payload.label = 'Private Message';
        url = 'send-message';
      } else {
        let teamMembers = this.teamMembers;
        let selfIndex = this.teamMembers.findIndex(user => parseInt(user) == parseInt(loggedUser.id));
        if(selfIndex >= 0) {
          teamMembers.splice(selfIndex, 1);
        }
        payload.receivers = teamMembers;
        payload.target_opened_chat = this.activeTeam;
        payload.target_opened_chat_type = 'team';
        payload.label = 'Group Message';
        payload.conv_title = this.conversationTitle;
        url = 'send-message-to-team';
      }

      if (this.one_to_one_user || this.chat_id) {
        payload.sender = loggedUser;
        payload.chat_id = this.chat_id;
        payload.to = this.one_to_one_user.toString();

        if(this.isWebSocketReady) {
          this.$webSocket.send(JSON.stringify({
            action: 'send_message',
            data: payload,
          }));
        }

        this.chatheadopen.message.body = this.msg_text;
        this.chatheadopen.message.created_at = new Date();
        this.chatheadopen.message.senderId = loggedUser.id.toString();
        this.chatheadopen.message.senderInfo = loggedUser;
        this.chatheadopen.message.sender = loggedUser;
      } else {
        this.chatheadopen.message.body = this.msg_text;
        this.chatheadopen.message.created_at = new Date();
        this.chatheadopen.message.senderId = loggedUser.id.toString();
        this.chatheadopen.message.senderInfo = loggedUser;
        this.chatheadopen.message.sender = loggedUser;

        // let us suppose that we will only send messages to group
        this.chats.push(payload);
        payload.sender = loggedUser.id.toString();
        this.msg_text = '';
        this.notifyKeyboardStatus();
        let data = await ApiService.post(`/v1/${url}`, payload).then(res => res.data);

        
        payload.msg_id = data.data.id;
        payload.team_id = this.activeTeam;

        if(this.isWebSocketReady) {
          this.$webSocket.send(JSON.stringify({
            action: 'send_message_in_group',
            data: payload,
          }));
        }

        this.teamChat = this.teamChat.map(item => {
            item.last_seen_msg_id = data.data.id;
            item.message.id = data.data.id;
          return item;
        });

        payload = {
          last_seen_msg_id: data.data.id,
          team_id: this.activeTeam
        }

        await ApiService.post('/v1/own-team-last-seen', payload).then(res => res.data).catch(err => console.log(err));
        this.$store.state.chat.chats = this.$store.state.chat.chats.map(item => {
          if(item.label == 'Group chat') {
            item.message.body = this.chats[this.chats.length - 1].body;
            item.message.id = payload.last_seen_msg_id;
            item.last_seen_msg_id = payload.last_seen_msg_id;
          }
          return item;
        }) 

      }

      if(!this.chatheadopen.message || this.chatheadopen.message == null) {
        this.chatheadopen.message = {
          body: this.msg_text,
          created_at: new Date(),
          seen: 1
        };
      }

      // commented out for now, we are supposing all the message will be group message. (no one-to-one msg now)
      // this.chats.push(payload);
      // payload.sender = loggedUser.id.toString();
      // this.msg_text = '';
      // this.notifyKeyboardStatus();
      // await ApiService.post(`/v1/${url}`, payload).then(res => res.data);
    },
    async sendConnectedTeamMessage() {
      let loggedUser = JSON.parse(localStorage.getItem('user'));
      let teamone = this.chatheadopen.from_team.team_members.map(member => member.user_id.toString());
      let teamTwo = this.chatheadopen.to_team.team_members.map(member => member.user_id.toString());
      let teamMembers = [...teamone, ...teamTwo];
      // let teamMembers = this.teamMembers;
      let selfIndex = teamMembers.findIndex(user => parseInt(user) == parseInt(loggedUser.id));
      if(selfIndex >= 0) {
        teamMembers.splice(selfIndex, 1);
      }

      let to_team_id = null;
      if(this.chatheadopen.from_team_id == this.activeTeam) {
        this.conversationTitle = this.chatheadopen.to_team ? this.chatheadopen.to_team.name : 'N/A';
        to_team_id = this.chatheadopen.to_team_id;
      } else {
        this.conversationTitle = this.chatheadopen.from_team ? this.chatheadopen.from_team.name : 'N/A';
        to_team_id = this.chatheadopen.from_team_id;
      }

      let teamConnectionId = null;
      if(this.chatheadopen.team_connection_id) {
        teamConnectionId = this.chatheadopen.team_connection_id;
      } else if(this.chatheadopen.team_private_chat && this.chatheadopen.team_private_chat.team_connection_id) {
        teamConnectionId = this.chatheadopen.team_private_chat.team_connection_id;
      } else {
        teamConnectionId = this.chatheadopen.id;
      }

      let payload = {
        support: false,
        to_team_id: to_team_id,
        from_team_id: this.activeTeam,
        sender: loggedUser.id,
        receivers: teamMembers,
        message: this.msg_text,
        body: this.msg_text,
        created_at: new Date(),
        senderId: loggedUser.id.toString(),
        senderInfo: loggedUser,
        target_opened_chat_type: 'connected-team',
        label: 'Connected',
        conv_title: this.conversationTitle,
        logo: this.chatheadopen.logo,
        // team_connection_id: this.chatheadopen.id,
        team_connection_id: teamConnectionId,
      };
      payload.target_opened_chat = payload.to_team_id;
      
      
      this.connectedTeamChats.push(payload);

      this.msg_text = '';
      this.notifyKeyboardStatus();
      
      let data = await ApiService.post(`/v1/send-message-team-to-team`, payload).then(res => res.data.data);
      payload.msg_id = data.id;
      payload.team_chat_id = data.team_chat_id;

      // this.$socket.emit('send_message_in_group', payload);
      if(this.isWebSocketReady) {
        this.$webSocket.send(JSON.stringify({
            action: 'send_message_in_group',
            data: payload,
          }));
      }

      this.chatheadopen.message = {
        body: payload.body,
        created_at: payload.created_at,
        sender: loggedUser,
        sender_id: loggedUser.id,
        team_connection_id: payload.team_connection_id,
        message: payload.body,
      };

      this.notify = this.chatheadopen.message;

      this.connectedTeam = this.connectedTeam.map(item => {
        if(item.id == this.chatheadopen.id) {
          item.last_seen_msg_id = data.id;
          item.message.id = data.id;
        }
        return item;
      });

      payload = {
        team_chat_id: data.team_chat_id,
        last_seen_msg_id: data.id
      }

      await ApiService.post('/v1/connected-team-last-seen', payload).then(res => res.data).catch(err => console.log(err));
      this.$store.state.chat.chats = this.$store.state.chat.chats.map(item => {
        if(item.message.team_chat_id == payload.team_chat_id) {
          item.message.body = this.connectedTeamChats[this.connectedTeamChats.length - 1].body;
          item.message.id = this.chatheadopen.message?.id;
          item.last_seen_msg_id = this.chatheadopen.message?.id;
        }
        return item;
      })
      

    },
    async sendPrivateMessage() {
      let loggedUser = JSON.parse(localStorage.getItem('user'));
      let url = "connected-send-private-message";
      let payload = {
        support: false,
        body: this.msg_text,
        message: this.msg_text,
        created_at: new Date(),
        senderId: loggedUser.id.toString(),
        senderInfo: loggedUser,
        target_opened_chat_type: 'private-chat',
        target_opened_chat: this.private_chat,
        receiver: this.chatheadopen.private_receiver_data.id.toString(),
        label: 'Private',
        conv_title: this.conversationTitle,
        logo: this.chatheadopen.logo,
        private_team_chat_id: this.chatheadopen.private_team_chat_id
      }
      payload.sender = loggedUser.id.toString();

      this.connectedTeamChats.push(payload);

      this.chatheadopen.message.body = this.msg_text;
      this.chatheadopen.message.created_at = new Date();
      this.chatheadopen.message.senderId = loggedUser.id.toString();
      this.chatheadopen.message.senderInfo = loggedUser;

      if(this.isWebSocketReady) {
        this.$webSocket.send(JSON.stringify({
          action: 'send_message',
          data: payload
        }));
      }


      payload.from_team_id = this.activeTeam;
      if(parseInt(this.activeTeam) == parseInt(this.chatheadopen.from_team_id)) {
        payload.to_team_id = this.chatheadopen.to_team_id;
      } else {
        payload.to_team_id = this.chatheadopen.from_team_id;
      }

      this.msg_text = '';
      this.notifyKeyboardStatus();

      await ApiService.post(`/v1/${url}`, payload).then(res => res.data);
    },
    notifyKeyboardStatus() {

      let loggedUser = JSON.parse(localStorage.getItem('user'));
      this.chatheadopen.message.seen = 1;
      let data = {
        type: this.chatheadopen.label,
        other_mate_id: this.chatheadopen.other_mate_id,
        typer_id: loggedUser.id,
        typer: loggedUser,
        team_connection_id: this.chatheadopen.id
      };

      if(data.type === 'Group chat') {
        data.members = this.teamMembers;
        data.team_id = this.activeTeam;
        data.typer_name = loggedUser.full_name;
      } else if(data.type == 'Connected Team') {
        let members = [];
        if(this.chatheadopen.to_team && this.chatheadopen.to_team.team_members) {
          this.chatheadopen.to_team.team_members.forEach(item => {
            members.push(item.user_id);
          });
        }
        if(this.chatheadopen.from_team && this.chatheadopen.from_team.team_members) {
          this.chatheadopen.from_team.team_members.forEach(item => {
            members.push(item.user_id);
          });
        }
        let ownIndex = members.findIndex(item => item == loggedUser.id);
        if(ownIndex >= 0) {
          members.splice(ownIndex, 1);
        }
        data.typer_name = loggedUser.full_name;
        data.members = members;
      } else {
        data.to = this.chatheadopen.other_mate_id.toString();
        data.typer_name = '';
      }
      if(this.msg_text && this.msg_text.length > 0) {
        data.status = 1;
        data.text = 'Typing...';
      } else {
        data.status = 0;
        data.text = '';
        data.typer_name = '';
        data.typer = '';
      }

      if(data.team_id || data.type == 'Connected Team') {
        data.members.forEach(item => {
          if(item != loggedUser.id) {
            data.to = item.toString();

            if(this.isWebSocketReady) {
              this.$webSocket.send(JSON.stringify({
                action: 'typing',
                data: data
              }));
            }
          }
        });
      } else {
        if(this.isWebSocketReady) {
          this.$webSocket.send(JSON.stringify({
            action: 'typing',
            data: data
          }));
        }
      }
    },
    unique(array) {
      return array.filter(function (el, index, arr) {
        return index == arr.indexOf(el);
      });
    },
    checkUnread: function (conv_id) {
      for (var i = 0; i < this.$store.state.chat.unread_records.length; i++) {
        if (this.$store.state.chat.unread_records[i].conv_id == conv_id) {
          return true;
        }
      }
      return false;
    },
    getUserImage(user_id) {
      for (var i = 0; i < this.$store.state.chat.user_info.length; i++) {
        // console.log(user_id,this.$store.state.chat.user_info[i].user_id);
        if (this.$store.state.chat.user_info[i].user_id == user_id) {
          return this.$store.state.chat.user_info[i].per_main_image_url + `?token=${this.token}`
        }
      }
      return '../../assets/info-img.png';
      // return '../../assets/r-join2.png';
    },
    getMsgDate(msg_date) {
      var success = "";
      var date = "";
      try {
        date = msg_date.toDate();
        success = "t";
        //return date;
      } catch (e) {
        success = "f";
        var seconds = msg_date.seconds
        date = new Date(seconds * 1000);
      }

      if (success == "t") {
        console.log("DateParse:success:", msg_date);
      } else {
        console.log("DateParse:fail:", msg_date);
      }

      return date;
    },
    findOneAndPushToFirst() {
      let recentIndex = this.chatHistory.findIndex(item => item == this.chatheadopen);
      if(recentIndex >= 0) {
        let data = this.chatHistory[recentIndex];
        this.chatHistory.splice(recentIndex, 1);
        this.chatHistory.unshift(data);
      }

      let teamIndex = this.teamChat.findIndex(item => item == this.chatheadopen);
      if(teamIndex >= 0) {
        let data = this.teamChat[teamIndex];
        this.teamChat.splice(teamIndex, 1);
        this.teamChat.unshift(data);
      }

      let connectIndex = this.connectedTeam.findIndex(item => item == this.chatheadopen);
      if(connectIndex >= 0) {
        let data = this.connectedTeam[connectIndex];
        this.connectedTeam.splice(connectIndex, 1);
        this.connectedTeam.unshift(data);
      }
    },
    getImage() {
      return this.InfoImg;
    },
    getConversationUserImage(id) {
      let imageObj = this.chatListedImage.find(item => item.user_id == id);
      if(imageObj) {
        return imageObj.logo;
      } else {
        return this.getImage();
      }
    },

    scrollBottom() {
        setTimeout(() => {
          const messages = document.getElementById('chat-messages');
          const messagesid = document.getElementById('messagesid');
          messages.scrollTop = messagesid.offsetTop - 10;
        }, 1000)
    },
    append(emoji) {
      this.msg_text += emoji.i
    },
    onSelectEmoji(emoji) {
      console.log(emoji)
      /*
        // result
        { 
            i: "😚", 
            n: ["kissing face"], 
            r: "1f61a", // with skin tone
            t: "neutral", // skin tone
            u: "1f61a" // without tone
        }
        */
    },
    handleReceiveMessage(res) {
      console.log('receive_message in chat', res);
      console.log(res, 'data from socket receive_message');
      const recieveMessage = {
        sender: res.data.senderInfo,
        created_at: res.data.created_at,
        updated_at: res.data.updated_at,
        body: res.data.body,
      };
      if(res.data.label == 'Connected') {
        this.notify = res;
      }
      if (this.chatTab === 'Connected') {
        if(!res.data.team_id) {
          recieveMessage.team_connection_id = res.data.team_connection_id;
          recieveMessage.id = res.data.msg_id;
          recieveMessage.team_chat_id = res.data.team_chat_id;
          if(this.connectedTeam) {
            this.connectedTeam = this.connectedTeam.map(item => {
              if(item.id == res.data.team_connection_id) {
                item.message.id = res.data.msg_id;
                item.message.body = res.data.body;
                item.message.team_chat_id = res.data.team_chat_id;
              }
              return item;
            });
          }
          this.connectedTeamChats = [...this.connectedTeamChats, recieveMessage];
          console.log('connected team chat ------ ', this.connectedTeamChats);
          this.notify = res;
        }
      } else {
        if(res.data.team_id) {
          recieveMessage.id = res.data.msg_id;
          if(this.teamChat) {
            this.teamChat = this.teamChat.map(item => {
              item.message.id = res.data.msg_id;
              item.message.body = res.data.body;
              item.message.team_id = res.data.team_id;
              return item;
            });
          }
          this.chats = [...this.chats, recieveMessage];
          this.newMessage = true;
        }
      }
    },
    handleReceiveNotification(res) {
      console.log('receive_notification in chat', res);
      if (res && res.data.type) {
        this.loadPageData();
      }
    },
    handleLisTyping(res) {
      console.log('lis_typing in chat', res);
      if(res.data.team_id) {
        if(this.chatHistory.length > 0) {
          this.chatHistory[0].typing_status = res.data.status;
          this.chatHistory[0].typing_text = res.data.text;
          this.chatHistory[0].typer_name = res.data.typer_name;
        }
        if(this.teamChat.length > 0) {
          this.teamChat[0].typing_status = res.data.status;
          this.teamChat[0].typing_text = res.data.text;
          this.teamChat[0].typer_name = res.data.typer_name;
        }
      } else {
        if(res.data.type == 'Connected Team') {
          let connectedTeamChat = this.connectedTeam.find(item => item.id == res.data.team_connection_id);
          if(connectedTeamChat) {
            connectedTeamChat.typing_status = res.data.status;
            connectedTeamChat.typing_text = res.data.text;
            connectedTeamChat.typer_name = res.data.typer_name;
          }
        } else {
          let teamPersonalChat = this.teamChat.find(item => item.other_mate_id == res.data.typer_id);
          if(teamPersonalChat) {
            teamPersonalChat.typing_status = res.data.status;
            teamPersonalChat.typing_text = res.data.text;
          }
          let recentChat = this.chatHistory.find(item => item.other_mate_id == res.data.typer_id);
          if(recentChat) {
            recentChat.typing_status = res.data.status;
            recentChat.typing_text = res.data.text;
          }
        }
      }
    },
  }
};

</script>

<style scoped lang="scss">
// start css for chat
.container {
  width: 100% !important;
}

.main-container {
  padding: 12px 15px !important;
  margin: 0px !important;

  .row {
    margin: 0px !important;
  }

  .col-12 {
    padding: 0px !important;
  
  }
}
@media (min-width: 1200px) {
  .container, .container-lg, .container-md, .container-sm, .container-xl {
    max-width: 100%;
  }
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
  padding: 20px 20px 20px 0px;
  width: 100%;
  border-radius: 10px;
  background-color: #ffffff;
  //box-shadow: 0px 0px 10px 1px rgba(63, 6, 17, 0.3);
  box-shadow: none !important;
  border: 2px solid #dddddd78 !important;
  height: calc(100vh - 92px) !important;
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
    height: 100%;
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
        padding-bottom: 0px;
        margin-top: 10px;
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
      //margin: 0 0rem;
      //justify-content: space-around;
      border-bottom: 2px solid #f2f2f2;
      @media (max-width: 991px) {
        padding-bottom: 10px;
        //margin: 0 6rem;
      }

      nav {
        //width: calc(100% - 44px);
        width: 100%;

        .nav-tabs {
          border: 0;
          display: flex;
          justify-content: space-around;
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
                  font-size: 12px;
                  font-weight: bold;
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
                  @media(min-width: 992px){
                    content: "";
                    width: 100%;
                    border-bottom: 2px solid #6059a7;
                    height: 2px;
                    background-color: red;
                    position: absolute;
                    left: 0;
                    bottom: -6px;
                  }
                }

                svg {
                  fill: #6059a7;
                }

                .category-name {
                  color: #6059a7;
                  &::after {
                    @media(max-width: 992px){
                      content: "";
                      width: 100%;
                      border-bottom: 2px solid #6059a7;
                      height: 3px;
                      position: absolute;
                      left: 0;
                      bottom: -10px;
                    }
                    @media(min-width: 767px) {
                      height: 2px;
                    }
                  }
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
        padding: 10px 0px 0px 8px;
        border-bottom: 1px solid #ececec;
        margin: 5px 0px 0px 0px;

        .top {
          position: relative;
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
              height: 50px;
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
        min-width: 300px;

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
              height: 50px;
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


      .rounded-circle {
        min-height: 40px;
        min-width: 40px;
      }

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
              height: 50px;
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
                overflow-y: scroll;
                height: 36px;
                width: 100%;
                border: 0;
                padding: 7px 54px 7px 40px;
                border-radius: 5px;
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
                border-radius: 8px;
                background-color: #eceaf5;
                resize: none;
                max-height: 70px;
                overflow-y: auto;
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
                top: 8px;
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

            button.js-msg-send {
              display: flex;
              justify-content: space-between;
              font-size: 15px;
              height: 36px;
              line-height: 28px;
              @media (max-width: 767px) {
                min-width: auto;

                .send-text {
                  display: none;
                }
              }
              //@media (max-width: 767px) {
                //  display: none;
                //}

              svg {
                width: 18px;
                float: left;

                .cls-1 {
                  fill: #fff;
                }

                .cls-2 {
                  fill: #8983be;
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
  border-radius: 8px;
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
  flex-shrink: 0;
  width:50%;
}
.w100{
  width: 100%;
}

.chat-message-left {
  margin-right: auto;
  width: 50%;
}

.chat-message-right {
  flex-direction: row-reverse;
  margin-left: auto;
  width: 50%;
}

@media (max-width: 600px) {
  .chat-message-left,
  .chat-message-right {
    width: 100%;
  }
}
.msg-right-created-at {
  bottom: -4px;
  right: 3rem;
}
.msg-left-created-at {
  bottom: -4px;
  left: 3rem;
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
    height: calc(100vh - 200px);
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
  margin-bottom: 0;
  margin-top: -14px;
  @media (min-width: 576px) {
    display: none;
  }
}
.chat-item-wrapper {
  //height: 500px;
  height: calc(100vh - 200px);
  @media (min-width: 410px) {
    height: calc(100vh - 200px);
  }
  @media (min-width: 576px) {
    height: calc(100vh - 200px);
  }
  @media (min-width: 768px) {
    height: calc(100vh - 210px);
  }
  @media (min-width: 992px) {
    height: calc(100vh - 310px);
  }
  @media (min-width: 1200px) {
    height: calc(100vh - 310px);
  }
  @media (min-width: 1920px) {
    height: calc(100vh - 310px);
  }
}
.chat-area {
  //min-height: 600px;
  min-height: calc(100vh - 210px);
  @media (min-width: 410px) {
    min-height: calc(100vh - 210px);
  }
  @media (min-width: 576px) {
    min-height: calc(100vh - 210px);
  }
  @media (min-width: 768px) {
    min-height: calc(100vh - 260px);
  }
  @media (min-width: 992px) {
    min-height: calc(100vh - 250px);
  }
  @media (min-width: 1200px) {
    min-height: calc(100vh - 250px);
  }
  @media (min-width: 1920px) {
    min-height: calc(100vh - 250px);
  }
}

.online-icon {
  flex-shrink: 0;
  width: 10px;
  height: 10px;
  display: block;
  background-color: #e42076;
  margin: 10px;
  border-radius: 50%;
  box-shadow: 0px 0px 4px 2px #e775a7;
}
.chat-messages {
  display: flex;
  flex-direction: column;
  overflow-y: auto;
  //max-height: 540px;
  max-height: calc(100vh - 310px);
  @media (min-width: 410px) {
    max-height: calc(100vh - 320px);
  }
  @media (min-width: 576px) {
    max-height: calc(100vh - 320px);
  }
  @media (min-width: 768px) {
    max-height: calc(100vh - 350px);
  }
  @media (min-width: 992px) {
    max-height: calc(100vh - 340px);
  }
  @media (min-width: 1200px) {
    max-height: calc(100vh - 340px);
  }
  @media (min-width: 1920px) {
    max-height: calc(100vh - 340px);
  }
}
.chat-request-chip {
  width: 20px;
  height: 20px;
  top: -12px;
  right: -8px;
  border-radius: 50%;
  color: #FFFFFF;
  display: flex;
  justify-content: center;
  align-items: center;
}
.chat-group {
  display: none;
  @media (min-width: 768px) {
    display: inherit;
  }
}
.selected-chat {
  background-color: #efefef;
  border-radius: 8px;
  padding-left: 4px;
}
.btn-mobile-back {
  margin-left: -20px;

  @media (min-width: 992px) {
    display: none !important;
  }

  @media (min-width: 577px) {
    position: absolute;
    top: 6px;
    left: -6px;
  }
}
.conv-head-logo {
  width: 35px;
  height: 35px;
  img {
    width: 35px !important;
    height: 35px !important;
    margin-left: 10px;
    @media (min-width: 576px) {
      width: 50px !important;
      height: 50px !important;
      margin-left: 0;
    }
  }
  @media (min-width: 576px) {
    width: 50px !important;
    height: 50px !important;
  }

  
}
.wrapper {
  position: relative;
  //display: inline-block;
}

.regular-input {
  padding: .5rem .5rem .5rem 2.3rem !important;
  border-radius: 3px;
  border: 1px solid #ccc;
  width: 20rem;
  height: 12rem;
  outline: none;
}

.regular-input:focus {
  box-shadow: 0 0 0 3px rgba(66,153,225,.5);
}

.emoji-invoker {
  position: absolute;
  top: .5rem;
  left: .5rem;
  width: 1.5rem;
  height: 1.5rem;
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.2s;
  padding: 0;
  background: transparent;
  border: 0;
  @media(max-width: 768px) {
    top: .5rem;
    left: .3rem;
  }
}
.emoji-invoker:hover {
  transform: scale(1.1);
}
.emoji-invoker > svg {
  fill: #b1c6d0;
}

.emoji-picker {
  position: absolute;
  z-index: 1;
  font-family: Montserrat;
  border: 1px solid #ccc;
  width: 15rem;
  height: 20rem;
  overflow: scroll;
  padding: 1rem;
  box-sizing: border-box;
  border-radius: 0.5rem;
  background: #fff;
  box-shadow: 1px 1px 8px #c7dbe6;
}
.emoji-picker__search {
  display: flex;
}
.emoji-picker__search > input {
  flex: 1;
  border-radius: 10rem;
  border: 1px solid #ccc;
  padding: 0.5rem 1rem;
  outline: none;
}
.emoji-picker h5 {
  margin-bottom: 0;
  color: #b1b1b1;
  text-transform: uppercase;
  font-size: 0.8rem;
  cursor: default;
}
.emoji-picker .emojis {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}
.emoji-picker .emojis:after {
  content: "";
  flex: auto;
}
.emoji-picker .emojis span {
  padding: 0.2rem;
  cursor: pointer;
  border-radius: 5px;
}
.emoji-picker .emojis span:hover {
  background: #ececec;
  cursor: pointer;
}
// end css for chat
</style>
