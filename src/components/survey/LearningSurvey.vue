<template>
  <section>
    <base-card>
      <h2>How was you learning experience?</h2>
      <form @submit.prevent="submitSurvey">
        <div class="form-control">
          <label for="name">Your Name</label>
          <input type="text" id="name" name="name" v-model.trim="enteredName" />
        </div>
        <h3>My learning experience was ...</h3>
        <!-- radio button poor -->
        <div class="form-control">
          <input
            type="radio"
            id="rating-poor"
            value="poor"
            name="rating"
            v-model="chosenRating"
          />
          <label for="rating-poor">Poor</label>
        </div>
        <!-- radio button average -->
        <div class="form-control">
          <input
            type="radio"
            id="rating-average"
            value="average"
            name="rating"
            v-model="chosenRating"
          />
          <label for="rating-average">Average</label>
        </div>
        <!-- radio button great -->
        <div class="form-control">
          <input
            type="radio"
            id="rating-great"
            value="great"
            name="rating"
            v-model="chosenRating"
          />
          <label for="rating-great">Great</label>
        </div>
        <p v-if="invalidInput">
          One or more input fields are invalid. Please check your provided data.
        </p>
        <p v-if="error">{{ error }}</p>
        <div>
          <base-button>Submit</base-button>
        </div>
      </form>
    </base-card>
  </section>
</template>

<script>
// import axios from 'axios';
export default {
  data() {
    return {
      enteredName: '',
      chosenRating: null,
      invalidInput: false,
      error: null,
    };
  },
  // emits: ['survey-submit'],
  methods: {
    submitSurvey() {
      // field is empty ,then invalidInput is true
      if (this.enteredName === '' || !this.chosenRating) {
        this.invalidInput = true;
        return;
      }
      this.invalidInput = false;
      // custom emit to App.vue (no need to emit custom event)
      //  this.$emit('survey-submit', {
      //       userName: this.enteredName,
      //       rating: this.chosenRating,
      //     });
      // SENDING DATA TO FIREBASE
      // axios
      // axios.post(
      //   'https://http-requests-with-vue-demo-default-rtdb.firebaseio.com/surveys.json',
      //   {
      //     name: this.enteredName,
      //     rating: this.chosenRating,
      //   }
      // );
      this.error = null;
      // sending http request
      fetch(
        'https://http-requests-with-vue-demo-default-rtdb.firebaseio.com/surveys.json',
        {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            name: this.enteredName,
            rating: this.chosenRating,
          }),
        }
      )
        .then((response) => {
          if (response.ok) {
            // successful
          } else {
            // server side error
            // 400 or 500 status (if data is not stringify )
            throw new Error('Could not save data');
          }
        })
        .catch((error) => {
          console.log(error);
          this.error = error.message;
        });

      // reset to initial value
      this.enteredName = '';
      this.chosenRating = null;
    },
  },
};
</script>

<style scoped>
.form-control {
  margin: 0.5rem 0;
}

input[type='text'] {
  display: block;
  width: 20rem;
  margin-top: 0.5rem;
}
</style>
