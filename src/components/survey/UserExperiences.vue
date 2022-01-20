<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences"
          >Load Submitted Experiences</base-button
        >
      </div>
      <p v-if="isLoading">Loading data...</p>
      <!-- invalid url -->
      <p v-else-if="!isLoading && error">{{ error }}</p>
      <!-- No data in firebase -->
      <p v-else-if="!isLoading && (!results || results.length === 0)">
        No stored data in database
      </p>
      <!-- Data is available in firebase -->
      <ul v-else-if="!isLoading && results && results.length > 0">
        <survey-result
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';

export default {
  // props: ['results'],
  components: {
    SurveyResult,
  },
  data() {
    return {
      results: [],
      isLoading: false,
      // initially no error
      error: null,
    };
  },
  methods: {
    loadExperiences() {
      this.isLoading = true;
      // reset error
      this.error = null;
      fetch(
        'https://http-requests-with-vue-demo-default-rtdb.firebaseio.com/surveys.json'
      )
        .then((response) => {
          if (response.ok) {
            // JSON Method :Parse data if it's in json format
            return response.json();
          }
        })
        .then((data) => {
          (this.isLoading = false),
            // access to data
            console.log(data);
          const tempResults = [];
          for (const id in data) {
            tempResults.push({
              id: id,
              name: data[id].name,
              rating: data[id].rating,
            });
          }
          this.results = tempResults;
        })
        .catch((error) => {
          this.isLoading = false;
          console.log(error);
          this.error = ' failed to fetch data -- Please try again later';
        });
    },
  },
  // loading data when components mount(without clicking button)
  mounted() {
    this.loadExperiences();
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
