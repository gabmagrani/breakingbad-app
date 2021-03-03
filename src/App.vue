<template>
 <div class="">
    <div class="max-w-md  py-8 pl-6" id="nav">
      <nav class="flex flex-row justify-between">
        <ul class=" flex flex-row space-x-4 justify-between ">
        <router-link class="font-bold text-gray-700 text-lg" to="/">Cards</router-link>
        
        <router-link class="font-bold text-gray-700 text-lg" to="/profile/1"
          >Profile</router-link
        >
        
        <router-link class="font-bold text-gray-700 text-lg" to="/episodes"
          >Episodes</router-link
        >
        </ul>
      </nav>
    </div>
    <div class=" mx-auto max-w-md justify-center ">
        <img src="/img/blogo.png" class="w-30 py-8 mx-auto justify-center ">
      </div>
    <router-view
      @CharacterSelected="characterSelected"
      @EpisodeSelected="episodeSelected"
    />
  </div>
</template>


<script>
export default {


  data() {
    return {
      char_id: "1",
    }
  },

  methods: {
    characterSelected(char_id) {
      console.log("App CharacterSelected:", char_id);
      this.char_id = char_id;
      this.$router.push({ name: 'Profile', params: { char_id } });
    },
    episodeSelected(episode_id) {
      console.log("App EpisodeSelected", episode_id);
    }
  },


}
</script>


<style>
#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
