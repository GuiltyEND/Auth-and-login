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
const email = ref("");
const password = ref("");
const isSignUp = ref(false);
const client = useSupabaseClient();

const signUp = async () => {
  const { user, error } = await client.auth.signUp({
    email: email.value,
    password: password.value,
  });
  console.log("user", user);
  console.log("error", error);
};

const login = async () => {
  const { user, error } = await client.auth.signIn({
    email: email.value,
    password: password.value,
  });
  console.log("user", user);
  console.log("error", error);
};

const user = useSupabaseUser();
onMounted(() => {
  watchEffect(() => {
    if (user.value) {
      navigateTo("/notes");
    }
  });
});

// const handleLogin = async () => {
//   try {
//     loading.value = true;
//     const { error } = await supabase.auth.signInWithOtp({
//       email: email.value,
//     });
//     if (error) throw error;
//     alert("Check your email for the login link!");
//   } catch (error) {
//     alert(error.error_description || error.message);
//   } finally {
//     loading.value = false;
//   }
// };
</script>