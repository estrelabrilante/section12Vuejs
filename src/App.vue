<template>
  <learning-survey @survey-submit="storeSurvey"></learning-survey>
  <user-experiences :results="savedSurveyResults"></user-experiences>
</template>

<script>
// Two main components
import LearningSurvey from './components/survey/LearningSurvey.vue';
import UserExperiences from './components/survey/UserExperiences.vue';

export default {
  components: {
    LearningSurvey,
    UserExperiences,
  },
  data() {
    // manage list of submitted survey
    return {
      savedSurveyResults: [],
    };
  },
  methods: {
    // methods for adding new results
    storeSurvey(surveyData) {
      const surveyResult = {
        name: surveyData.userName,
        rating: surveyData.rating,
        id: new Date().toISOString(),
      };
      this.savedSurveyResults.push(surveyResult);
      console.log(surveyResult);
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}

html {
  font-family: sans-serif;
}

body {
  margin: 0;
}
</style>
