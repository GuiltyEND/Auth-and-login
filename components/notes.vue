<template>
  <div>
    <p>Hello</p>
  </div>
</template>

<script>
const user = useSupabaseUser();
onMounted(() => {
  watchEffect(() => {
    if (!user.value) {
      navigateTo("/");
    }
  });
});
</script>