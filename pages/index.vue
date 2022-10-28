<template>
  <div>
    <form
      @submit.prevent="() => (isSignUp ? signUp() : login())"
      class="flex flex-col gap-2"
    >
      <input
        type="email"
        placeholder="Email"
        v-model="email"
        class="p-2 bg-gray-600 text-white rounded"
      />
      <input
        type="password"
        placeholder="Password"
        v-model="password"
        class="p-2 bg-gray-600 text-white rounded"
      />
      <button type="submit" class="p-2 text-white bg-green-500 rounded">
        <span v-if="isSignUp">Sign up</span>
        <span v-else>Login</span>
      </button>
    </form>
    <button
      @click="isSignUp = !isSignUp"
      class="w-full mt-8 text-sm text-center underline text-slate-300"
    >
      <span v-if="isSignUp">Have an account? Log in instead</span>
      <span v-else>Create a new account</span>
    </button>
  </div>
</template>

<script setup >
definePageMeta({
  middleware: "auth",
  layout: "autorization",
});

const supabase = useSupabaseClient();

const email = ref("");
const password = ref("");
const isSignUp = ref(false);

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
      navigateTo("/notes");
    }
  });
});
</script>