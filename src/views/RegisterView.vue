<template>
  <ContentBase>
    <div class="row div_register">
      <div class="col-3" >
        <form @click.prevent="register">
          <div class="mb-3">
            <label for="username" class="form-label">用户名</label>
            <input
              v-model="username"
              type="text"
              class="form-control"
              id="username"
            />
          </div>
          <div class="mb-3">
            <label for="userpwd" class="form-label">密码</label>
            <input
              v-model="password"
              type="password"
              class="form-control"
              id="userpwd"
            />
          </div>
          <div class="mb-3">
            <label for="password_confirm" class="form-label">确认密码</label>
            <input
              v-model="password_confirm"
              type="password"
              class="form-control"
              id="password_confirm"
            />
          </div>
          <div class="error_message">{{ error_message }}</div>
          <button type="submit" class="btn btn-primary">注册</button>
        </form>
      </div>
    </div>
  </ContentBase>
</template>
 
<script>
// @ is an alias to /src
import ContentBase from "../components/ContentBase.vue";
import { ref } from "vue";
import { useStore} from "vuex";
import $ from "jquery"
import router from "vuex"

export default {
  name: "RegisterView",
  components: {
    ContentBase,
  },
  setup(){
    const store = useStore();
    let username = ref('');
    let password = ref('');
    let password_confirm = ref('');
    let error_message = ref('');

    const register = () => {
      $.ajax({
        url: "https://app165.acapp.acwing.com.cn/myspace/user/",
        type: "POST",
        data: {
          username: username.value,
          password :password.value,
          password_confirm: password_confirm.value,
        },
        success(resp){
          if(resp.result === "success"){
              store.dispatch("login", {
                username : username.value,
                password : password.value,
                success(){
                  router.push({ name: 'userlist' });
                },
                error() {
                  error_message.value = "系统异常";
                }
             });
          }else {
            error_message.value = resp.result;
          }
        }
      });
      // console.log(username, password, repassword);
    };

    return {
      username,
      password,
      password_confirm,
      register,
      error_message,
    }
  }
};
</script>


<style scoped>
.div_register{
  display: flex;
  justify-content: center;
}
</style>