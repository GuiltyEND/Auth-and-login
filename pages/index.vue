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
      <login 
        :email="email" 
        :password="password"
      />
      <register 
        :email="email"
        :password="password" 
      />
    </div>
  </div>
</template>

<script setup>
import login from '../components/login.vue'
import register from '../components/register.vue'



const supabase = useSupabaseClient();

definePageMeta({
  middleware: "auth",
  layout: "autorization",
});



const isSignUp = ref(false);
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