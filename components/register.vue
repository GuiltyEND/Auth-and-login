<template>
  <button
    type="submit"
    class="p-2 text-white bg-green-500 rounded"
    @click="signUp"
  >
    <span>Sign up</span>
  </button>
  <button
    class="w-full mt-8 text-sm text-center underline text-slate-300"
    @click="changeComponent"
  >
    <span>Have an account? Log in instead</span>
  </button>
</template>
<script>

export default {
  props:{email:String,password:String},
  emits:['changeComponent'],
  methods: {
    changeComponent() {
      this.$emit('changeComponent','Login')
    },
    signUp(){
      const supabase = useSupabaseClient();
      
      supabase.auth.signUp({
      email: this.email,
      password: this.password,
    });
    }
  }
}
</script>