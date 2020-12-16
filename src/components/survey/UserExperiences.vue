<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click='loadExperience'>Load Submitted Experiences</base-button>
      </div>
      <p v-if='isLoading' :style='styleObject'>Loading ...</p>
      <p v-else-if='!isLoading && error'>{{error}}</p>

      <p v-else-if='!isLoading && (!results || results.length === 0)'>No store experience found. Add some</p>
      <ul v-else>
        <survey-result
          v-for='result in results'
          :key='result.id'
          :name='result.name'
          :rating='result.rating'
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
    SurveyResult
  },
  data() {
    return {
      results: [],
      isLoading: false,
      error: null,
      styleObject: {
        color: 'red',
        fontSize: '43px',
        border: 'blue 1px solid'
      }
    };
  },
  methods: {
    loadExperience() {
      this.isLoading = true;
      this.error= null;
      fetch('https://max-vue-http-demo-33983-default-rtdb.europe-west1.firebasedatabase.app/surveys.json')
        .then((response) => {
          if (response.ok) {
            return response.json();
          }
        })
        .then((data) => {
        this.isLoading = false;
        // console.log(data);
        const results = [];
        for (const id in data) {
          results.push({ id: id, name: data[id].name, rating: data[id].rating });
        }
        this.results = results;
      })
        .catch(()=>{
          // console.log(error);
          this.isLoading = false;
          this.error  = "NU merge...Incearca mai tarziu"

        })

    }
  },
  mounted() {
    this.loadExperience();
  }
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>