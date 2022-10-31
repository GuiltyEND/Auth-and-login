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
        <button
          type="submit"
          class="p-2 text-white bg-green-500 rounded"
        >
          <span v-if="isSignUp">Sign up</span>
          <span v-else>Login</span>
        </button>
      </form>
      <button
        class="w-full mt-8 text-sm text-center underline text-slate-300"
        @click="isSignUp = !isSignUp"
      >
        <span v-if="isSignUp">Have an account? Log in instead</span>
        <span v-else>Create a new account</span>
      </button>
    </div>
  </div>
</template>

<script setup>


definePageMeta({
  middleware: "auth",
  layout: "autorization",
});

const supabase = useSupabaseClient();


const isSignUp = ref(false);
const email = ref("");
const password = ref("");

const login = async () => {
  try {
    const { error } = await supabase.auth.signInWithPassword({
      email: email.value,
      password: password.value,
    });
  } catch {
    if (error) throw error;
    alert(error.message);
  }
};
const signUp = async () => {
  try {
    const { error } = await supabase.auth.signUp({
      email: email.value,
      password: password.value,
    });
  } catch (error) {
    alert(error.error_description || error.message);
  }
};





const user = useSupabaseUser();
onMounted(() => {
  watchEffect(() => {
    if (user.value) {
      navigateTo("/account");
    }
  });
});
</script>