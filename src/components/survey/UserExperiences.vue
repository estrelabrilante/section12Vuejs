<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences"
          >Load Submitted Experiences</base-button
        >
      </div>
      <ul>
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
    };
  },
  methods: {
    loadExperiences() {
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
