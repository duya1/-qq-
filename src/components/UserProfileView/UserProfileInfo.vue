<template>
  <div class="card">
    <div class="card-body">
      <div class="row">
        <div class="col-3" style="display: flex; justify-content: center; align-items: center;">
          <img
            :src="user.photo"
            class="img-fluid"
            alt=""
          />
        </div>
        <div class="col-9">
          <div class="name">{{ user.username }}</div>
          <div class="fans">粉丝数:{{ user.followerCount }}</div>
          <button
            v-if="!user.is_followd"
            v-on:click="follow"
            type="button"
            class="btn btn-secondary btn-sm"
          >
            +关注
          </button>
          <button
            v-if="user.is_followd"
            v-on:click="unfollow"
            type="button"
            class="btn btn-secondary btn-sm"
          >
            取消关注
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import $ from "jquery"
import {useStore} from "vuex"

export default {
  name: "UserProfileInfo",
  props: {
    user: {
      type: Object,
      require: true,
    },
  },
  setup(props, context) {
    
    const store = useStore();
    const unfollow = () => {
      $.ajax({
        url: "https://app165.acapp.acwing.com.cn/myspace/follow/",
        type: "POST",
        data: {
          target_id : props.user.id
        },
        headers:{
          'Authorization' : 'Bearer ' + store.state.user.access
        },
        success(resp){
          if(resp.result === "success"){
            context.emit("unfollow");
            // store.state.user.is_followd = false;
          }
        }
      })
    };

    const follow = () =>{
      $.ajax({
        url: "https://app165.acapp.acwing.com.cn/myspace/follow/",
        type: "POST",
        data: {
          target_id : props.user.id
        },
        headers:{
          'Authorization' : 'Bearer ' + store.state.user.access
        },
        success(resp){
          if(resp.result === "success"){
            context.emit("follow");
            // store.state.user.is_followd = true;
          }
        }
      })
    }

    return {
      follow,
      unfollow,
    };
  },
};
</script>

<style scoped>
img {
  border-radius: 50%;
}

.name {
  font-weight: bold;
}

.fans {
  font-size: 10px;
}

.btn {
  padding: 2px 4px;
  font-size: 12px;
}
</style>