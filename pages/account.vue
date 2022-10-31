<template>
  <div class="bg-black min-h-screen py-32 px-10">
    <div class="p-10 rounded-lg lg:w-1/2 mx-auto">
      <form 
        class="flex flex-col gap-2" 
        @submit.prevent="updateProfile"
      >
        <div class="mb-5">
          <label 
            class="block mb-2 text-gray-500 font-bold"
            for="email"
          >Email</label>
          <input
            id="email"
            class="p-3 w-full bg-gray-600 text-white rounded"
            type="text"
            :value="user.email"
            disabled
          >
        </div>
        <div class="mb-10">
          <label 
            class="block mb-2 text-gray-500 font-bold" 
            for="username"
          >Username</label>
          <input
            id="username"
            v-model="username"
            minlength="4"
            class="p-3 bg-gray-600 text-white w-full rounded"
            type="text"
          >
        </div>

        <div>
          <input
            style="cursor: pointer"
            type="submit"
            class="w-full mt-2 text-sm text-center underline text-slate-300"
            :value="loading ? 'Loading ...' : 'Change name'"
            :disabled="loading"
          >
        </div>

        <div>
          <button
            class="p-2 text-white bg-green-500 rounded"
            :disabled="loading"
            @click="signOut"
          >
            Sign Out
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient();

const loading = ref(true);
const username = ref("");
const user = useSupabaseUser();
const { data } =  await supabase
  .from("profiles")
  .select("username")
  .eq("id", user.value.id)
  .single();
if (data) {
  username.value = data.username;
}
loading.value = false;
const updateProfile = async () => {
  try {
    loading.value = true;
    const user = useSupabaseUser();
    const updates = {
      id: user.value.id,
      username: username.value,
      updated_at: new Date(),
    };
    const { error } = await supabase.from("profiles").upsert(updates, {
      returning: "minimal",
    });
    if (error) throw error;
  } catch (error) {
    alert(error.message);
  } finally {
    loading.value = false;
  }
};
const signOut = async () => {
  try {
    loading.value = true;
    const { error } = await supabase.auth.signOut();
    if (error) throw error;
    user.value = null;
  } catch (error) {
    alert(error.message);
  } finally {
    loading.value = false;
  }
};
definePageMeta({
  middleware: "auth",
});

onMounted(() => {
  watchEffect(() => {
    if (!user.value) {
      navigateTo("/");
    }
  });
});
</script>
