<template>
  <button
    type="submit"
    class="p-2 text-white bg-green-500 rounded"
    @click="login"
  >
    <span>Login</span>
  </button>
  <button
    class="w-full mt-8 text-sm text-center underline text-slate-300"
    @click="changeComponent"
  >
    <span>Create a new account</span>
  </button>
</template>
<script>

export default {
  props:{email:String,password:String},
  emits:['changeComponent'],
  methods: {
    changeComponent() {
      this.$emit('changeComponent', 'Register')
    },
    login() {
      const supabase = useSupabaseClient();

      supabase.auth.signInWithPassword({
      email: this.email,
      password: this.password,
    });
    } 
  } 
}

</script>