<template>
  <div class="card card-style">
    <div class="card-body">
      <div class="mb-3">
        <label for="edit-post" class="form-label">编辑区</label>
        <textarea
          v-model="content"
          class="form-control"
          id="edit-post"
          rows="3"
        ></textarea>
        <button v-on:click="send" type="button" class="btn btn-primary btn-sm">
          发帖
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import $ from "jquery";
import { useStore } from "vuex";

export default {
  name: "UserProfileWirte",

  setup(props, context) {
    const store = useStore();
    let content = ref("");
    const send = () => {
      $.ajax({
        url: 'https://app165.acapp.acwing.com.cn/myspace/post/',
        type: "POST",
        data:{
          content: content.value,
        },
        headers: {
        'Authorization' : "Bearer " + store.state.user.access,
        },
        success(resp){
          if(resp.result === "success" && content.value != ""){
              context.emit("send", content.value);
              content.value = "";
          }
        },
      });
    };

    return {
      content,
      send,
    };
  },
};
</script>

<style scoped>
.card-style {
  margin-top: 20px;
}

button {
  margin-top: 10px;
}
</style>