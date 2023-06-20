<template>
  <ContentBase>
    <div class="card user-card" v-for="user in users" :key="user.id" @click="open_user_profile(user.id)">
      <div class="card-body">
        <div class="row">
          <div class="col-1">
            <img class="img-fluid" :src="user.photo" alt="" />
          </div>
          <div class="col-11">
            <div class="username">{{ user.username }}</div>
            <div class="userfans">粉丝数:{{ user.followerCount }}</div>
          </div>
        </div>
      </div>
    </div>
  </ContentBase>
</template>
 
<script>
// @ is an alias to /src
import ContentBase from "../components/ContentBase.vue";
import $ from "jquery";
import { ref } from "vue";
import router from "@/router/index";
import { useStore } from "vuex";

export default {
  name: "UserList",
  components: {
    ContentBase,
  },
  setup() {
    let users = ref([]);
    const store = useStore();

    $.ajax({
      url: "https://app165.acapp.acwing.com.cn/myspace/userlist/",
      type: "get",
      success(resp) {
        users.value = resp;
      },
    });

    const open_user_profile = userId => {
      if(store.state.user.is_login)
        router.push({name: "userprofile", params: {userId}});
      else 
        router.push({name: "login"});
    }

    return {
      users,
      open_user_profile,
    };
  }
};
</script>

<style scoped>
.user-card {
  margin-bottom: 10px;
  cursor: pointer;
}

.user-card:hover {
  box-shadow: 2px 2px 10px lightgray;
}

img {
  border-radius: 50%;
}

.username {
  font-size: 20px;
  font-weight: bold;
  height: 50%;
}

.userfans {
  font-size: 16px;
  color: gray;
  height: 50%;
}
</style>