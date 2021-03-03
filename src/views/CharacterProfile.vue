<template>
  <div class="container p-5 mx-auto my-5">
    <div class="md:flex no-wrap md:-mx-2">
      <!-- Left Side -->
      <div class="w-full md:w-3/12 md:mx-2">
        <!-- Profile Card -->
        <div class="p-3 bg-white border-t-4 border-green-400">
          <div class="overflow-hidden image">
            <img class="w-full h-auto mx-auto" :src="character.img" alt="" />
          </div>
          <h1 class="my-1 text-xl font-bold leading-8 text-gray-900">
            {{ character.name }}
          </h1>
          <h3 class="leading-6 text-gray-600 font-lg text-semibold">
            {{ character.nickname }}
          </h3>
        </div>
        <!-- End of profile card -->
        <div class="my-4"></div>
      </div>
      <!-- Right Side -->
      <div class="w-full h-64 mx-2 md:w-9/12">
        <!-- Profile tab -->
        <!-- About Section -->
        <div class="p-3 bg-white rounded-sm shadow-sm">
          <div
            class="flex items-center space-x-2 font-semibold leading-8 text-gray-900"
          >
            <CharacterQuotes :char_id="char_id" />
          </div>
        </div>
        <!-- End of about section -->

        <div class="my-4"></div>

        <!-- Experience and education -->
        <div class="p-3 bg-white rounded-sm shadow-sm">
          <div class="grid grid-cols-2">
            <div>
              <div
                class="flex items-center mb-3 space-x-2 font-semibold leading-8 text-gray-900"
              >
                <CharacterEpisodes :char_id="char_id" />
              </div>
            </div>
          </div>
          <!-- End of Experience and education grid -->
        </div>
        <!-- End of profile tab -->
      </div>
      <!-- <CharacterQuotes :char_id="char_id" /> -->
      <!-- <CharacterEpisodes :char_id="char_id" /> -->
    </div>
  </div>
</template>

<script>

/* Profile Component

  Purpose:  display Information about a single character from the tv show "Breaking Bad",
            it watches for changes of the char_it and updates the character data from the backend, if needed.

  Requires: the char_id of the character passed in via props.

  Events:   none

*/

import CharacterEpisodes from '../components/CharacterEpisodes.vue'
import CharacterQuotes from '../components/CharacterQuotes.vue'

export default {
  name: 'CharacterProfile',

  props: {
    char_id: String,
  },

  components: {
    CharacterEpisodes,
    CharacterQuotes,
  },

  data() {
    return {
      character: {},
    }
  },

  mounted() {

    this.loadCharacterByID(this.char_id)

  },

  watch: {

    char_id(new_id, old_id) {
      if (new_id && new_id !== old_id) {
        this.loadCharacterByID(new_id);
      }
    }
  },

  methods: {

    async loadCharacterByID(charID) {

      // protect the function from invalid input data
      if (!charID) {
        return;
      }

      // get data from the backend
      const { data, error } = await to(getCharacterByID(charID));

      if (error) {
        return;
      }

      // update the local state 
      this.character = data[0]; //data contains an array with one element! 

    }
  }
}


// ----------- this code is here to simplify combining components by making each component independent -----------------
// In a real app this code should be imported from a single file that is handling all the communication with the backend

const url = "https://www.breakingbadapi.com/api/"

function getCharacterByID(charID) {

  return fetch(url + "characters/" + charID);

}

/*
The 'to' function is a helper function that abstracts away the handling of promises. 
It accepts a promise as an argument, i.e. returned from a call to the fetch api 
and 'thens' down into the json and the data content.
*/

function to(promise) {
  return promise.then(response => response.json())
    .then(data => ({ data, error: null }))
    .catch(error => ({ data: null, error }));
}

</script>
