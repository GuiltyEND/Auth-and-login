<template>
  <div>
    <form @submit.prevent="() => (isSignUp ? signUp() : login())">
      <input type="email" placeholder="Email" v-model="email" />
      <input type="password" placeholder="Password" v-model="password" />
      <button type="submit">
        <span v-if="isSignUp">Sign up</span>
        <span v-else>Login</span>
      </button>
    </form>
    <button @click="isSignUp = !isSignUp">
      <span v-if="isSignUp">Have an account? Log in instead</span>
      <span v-else>Create a new account</span>
    </button>
  </div>
</template>

<script setup >
definePageMeta({
  middleware: "auth",
});

const supabase = useSupabaseClient();

const email = ref("");
const password = ref("");
const isSignUp = ref(false);

const login = async () => {
  const { error } = await supabase.auth.signInWithPassword({
    email: email.value,
    password: password.value,
  });
  if (error) throw error;
};

const signUp = async () => {
  const { error } = await supabase.auth.signUp({
    email: email.value,
    password: password.value,
  });
  if (error) throw error;
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