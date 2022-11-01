<template>
  <div class="bg-black min-h-screen py-32 px-10">
    <div class="p-10 rounded-lg lg:w-1/2 mx-auto">
      <form
        class="flex flex-col gap-2"
        @submit.prevent="() => (isSignUp ? signUp() : login())"
      >
        <input
          v-model="email"
          minlength="5"
          type="email"
          placeholder="Email"
          class="p-2 bg-gray-600 text-white rounded"
        >
        <input
          v-model="password"
          minlength="7"
          type="password"
          placeholder="Password"
          class="p-2 bg-gray-600 text-white rounded"
        > 
      </form>
      <keep-alive>
        <component 
          :is="currentlyAuthComponent"
          @change-component='changeComponent'
          :password="password" 
          :email="email">
        </component>
      </keep-alive>
    </div>
  </div>
</template>
<script>
import Login from '../components/login.vue'
import Register from '../components/register.vue'

export default {
    components: {
    Login,
    Register
  },
  data() {
    return {
      currentlyAuthComponent:'Login'
    }
  },

  methods: {
    changeComponent(component) {
    this.currentlyAuthComponent = component
    }
  } 
}
</script>
<script setup>

const supabase = useSupabaseClient();

definePageMeta({
  middleware: "auth",
  layout: "autorization",
});

const email = ref("");
const password = ref("");
const user = useSupabaseUser();

onMounted(() => {
  watchEffect(() => {
    if (user.value) {
      navigateTo("/account");
    }
  });
});
</script>