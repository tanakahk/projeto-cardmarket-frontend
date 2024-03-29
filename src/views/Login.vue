<template>
  <div class="container" @click="modalClose">
    <div class="content-container">
      <div class="content">
        <h1>Login</h1>
        <div id="userContainer" v-if="!next">
          <div ref="user">
            <input
              class="input-text"
              ref="userInput"
              v-model="username"
              type="text"
              placeholder="user..."
              @keyup="userHandler"
              autocomplete="off"
              autofocus
            />
          </div>
          <button ref="userBtn" @click="nextHandler" class="button">Next</button>
        </div>
        <div id="passContainer" style="display: fixed" v-else>
          <div ref="pass">
            <input
              class="input-text"
              ref="passInput"
              v-model="password"
              type="password"
              placeholder="senha..."
              @keyup="passHandler"
            />
          </div>
          <button ref="passBtn" @click="back" class="button">Back</button>
          <button ref="passBtn" @click="login" class="button">Enter</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import useAuth from '@/modules/auth';
import {
 defineComponent, Ref, ref, reactive, toRefs,
} from 'vue';
import { useRouter } from 'vue-router';

export default defineComponent({
  setup() {
    const auth = useAuth();
    const router = useRouter();
    const user: Ref<HTMLElement | null> = ref(null);
    const userInput: Ref<HTMLElement | null> = ref(null);
    const userBtn: Ref<HTMLElement | null> = ref(null);
    const pass: Ref<HTMLElement | null> = ref(null);
    const passInput: Ref<HTMLElement | null> = ref(null);
    const passBtn: Ref<HTMLElement | null> = ref(null);
    const next = ref(false);
    const state = reactive({
      username: '',
      password: '',
    });

    const login = async () => {
      if (state.username && state.password) {
        const res = await auth.actions.login(state.username, state.password);
        if (res) {
          router.push({ name: 'Home' });
        }
      }
    };

    const back = () => {
      next.value = false;
    };

    const nextHandler = () => {
      if (state.username) {
        next.value = true;
      }
    };

    const userHandler = (e: KeyboardEvent) => {
      if (e.key === 'Enter' && state.username) {
        next.value = true;
      }
    };

    const passHandler = (e: KeyboardEvent) => {
      if (e.key === 'Enter' && state.username && state.password) {
        login();
      }
    };

    return {
      ...toRefs(state),
      login,
      userHandler,
      passHandler,
      user,
      userInput,
      userBtn,
      pass,
      passInput,
      passBtn,
      next,
      nextHandler,
      back,
    };
  },
});
</script>

<style scoped>
.container {
  display: flex;
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
}
.content-container {
  position: relative;
  background-image: url(../assets/BackgroundLandscape.png);
  background-size: 100% 100%;
  max-width: 80%;
  min-width: 500px;
  height: 250px;
  margin: auto;
  padding: 40px;
  border: 1px solid #888;
  border-radius: 20px;
}
.content {
  display: flex;
  flex-flow: column nowrap;
  justify-content: center;
  height: 100%;
  align-items: center;
}
.input-text {
  margin-top: 5px;
  display: fixed;
  position: relative;
  top: 0;
  left: 0;
  font-size: 20px;
  border-radius: 20px;
  width: 200px;
  padding: 5px 10px 5px 10px;
}
.button {
  width: 100px;
  margin: 40px 0 10px 0;
  padding: 6px;
  border-radius: 10px;
  font-size: 22px;
  background-color: #0071e3;
  color: white;
}
</style>
