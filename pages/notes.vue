<template>
  <div>
    <div>Hello wolrds</div>
    <button @click="client.auth.signOut()"></button>
  </div>
</template>

<script setup>
const client = useSupabaseClient();

const user = useSupabaseUser();
definePageMeta({
  middleware: "auth",
});

onMounted(() => {
  watchEffect(() => {
    if (user.value) {
      navigateTo("/");
    }
  });
});
</script>