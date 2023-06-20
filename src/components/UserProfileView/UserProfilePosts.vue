<template>
  <div class="card">
    <div class="card-body">
      <div v-for="post in posts.posts" :key="post.id">
        <div class="card" style="margin-bottom: 10px">
          <div class="card-body">
            {{ post.content }}
            <button v-if="is_me" @click="delete_post(post.id)" type="button" class="btn btn-dark">删除</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { useStore } from 'vuex';
import {computed} from "vue"
import $ from "jquery"

export default {
  name: "UserProfilePosts",
  props: {
    posts: {
      type: Object,
      require: true,
    },
    user:{
      type: Object,
      require: true,
    }
  },
  setup(props, context) {
    const store = useStore();
    let is_me = computed( () => store.state.user.id === props.user.id)
    
    const delete_post = (post_id) =>{
      
      $.ajax({
        url: "https://app165.acapp.acwing.com.cn/myspace/post/",
        type: "DELETE",
        data: {
          post_id
        },
        headers:{
          'Authorization' : 'Bearer ' + store.state.user.access,
        },
        success(resp){
          if(resp.result === "success"){
            context.emit("delete_post", post_id);
          }
        }
      })
    }
    
    return {
      is_me,
      delete_post,
    }
  }
};
</script>

<style scoped>
button{
  float: right;
}
</style>