<template>
  <div >
    <AdminLayout>
      <router-view></router-view>
    </AdminLayout>

  </div>
</template>

<script>
import Footer from "@/components/admin/layout/Footer.vue";
export default {
  components: {Footer},
  
  data() {
    return {
      user: {},
    };
  },

  computed: {
    isWebSocketReady() {
      return this.$webSocket.readyState === 1;
    }
  },

  mounted() {
    console.log('admin system mounted');
    let loggedUser = JSON.parse(localStorage.getItem('user'));

    if(loggedUser) {
      if(this.isWebSocketReady) {
        this.$webSocket.send(JSON.stringify({
          action: 'ping',
          user_id: loggedUser.id
        }));
      }

      this.$webSocket.onmessage = ($event) => {
        let res = JSON.parse($event.data);
  
        if(res.event == 'ping_success') {
          console.log('ping success', res);
          if (res && res.data.online_users) {
            this.$store.state.chat.online_users = res.data.online_users;
          }
        }
      };

      this.$webSocket.onclose = () => {
        console.log('socket closed');
      };
    }
    
  },
};
</script>

<style lang="scss" scoped>

</style>
