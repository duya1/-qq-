<template>
  <ContentBase>
    <div class="row">
      <div class="col-3">
        <UserProfileInfo @follow="follow" @unfollow="unfollow" :user="user" />
        <UserProfileWrite  v-if="is_me" @send="send"  />
      </div>
      <div class="col-9">
        <UserProfilePosts :user="user" :posts="posts" @delete_post="delete_post"/>
      </div>
    </div>
  </ContentBase>
</template>
 
<script>
// @ is an alias to /src

import ContentBase from "../components/ContentBase.vue";
import UserProfileInfo from "../components/UserProfileView/UserProfileInfo.vue";
import UserProfilePosts from "../components/UserProfileView/UserProfilePosts.vue";
import UserProfileWrite from "../components/UserProfileView/UserProfileWrite.vue";
import { reactive } from "vue";
import { useRoute } from "vue-router";
import $ from "jquery"
import { useStore } from "vuex";
import {computed} from "vue"

export default {
  name: "UserProfileView",
  components: {
    ContentBase,
    UserProfileInfo,
    UserProfilePosts,
    UserProfileWrite,
  },
  setup() {
    const route = useRoute();
    const userId = parseInt(route.params.userId);
    const user = reactive({});
    const posts = reactive({});

    const store = useStore();
    $.ajax({
      url: "https://app165.acapp.acwing.com.cn/myspace/getinfo/",
      type: "GET",
      data :{
        user_id: userId
      },
      headers: {
        'Authorization' : "Bearer " + store.state.user.access
      },
      success(resp){
        user.id = resp.id;
        user.username = resp.username;
        user.photo = resp.photo;
        user.followerCount = resp.followerCount;
        user.is_followd = resp.is_followd;
      }
    });

    $.ajax({
      url: "https://app165.acapp.acwing.com.cn/myspace/post/",
      type: "GET",
      data :{
        user_id: userId
      },
      headers: {
        'Authorization' : "Bearer " + store.state.user.access
      },
      success(resp){
        posts.posts = resp;
        posts.count = resp.length;
      }
    });

    const follow = () => {
      if (user.is_followd) return;
      user.is_followd = true;
      user.followerCount++;
    };

    const unfollow = () => {
      if (!user.is_followd) return;
      user.is_followd = false;
      user.followerCount--;
    };

    const send = (content) => {
      posts.count++;
      posts.posts.unshift({
        id: posts.count,
        userid: 1,
        content: content,
      });
    };

    const delete_post = (post_id) => {
      posts.posts = posts.posts.filter(post => post.id !== post_id);
      posts.count = posts.posts.length;
    }

    const is_me = computed(() => userId === store.state.user.id);

    return {
      user,
      follow,
      unfollow,
      posts,
      send,
      is_me,
      delete_post
    };
  },
};
</script>

<style scoped>
</style>