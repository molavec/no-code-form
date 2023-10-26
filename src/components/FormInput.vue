<script setup lang="ts">
import { ref} from 'vue';

const WEBHOOK = import.meta.env.VITE_MAKE_WEBHOOK;
// const WEBHOOK = 'https://example.com'
const GOOGLE_SHEET_URL = 'https://docs.google.com/spreadsheets/d/1RP9BFhRAo-8fgwbjb9FabeEokeaDcTpk82blEcXc4Z0/edit?usp=sharing';
const isSubmitted = ref(false);
const form = ref(null);


const handleSubmit = () => {
    isSubmitted.value = !isSubmitted.value;
    if (form.value) {
      const data = new FormData(form.value);
      
      console.log(data);
  
      fetch(
        WEBHOOK,
        {
          method: 'POST',
          body: data,
        },
      )
      .then( response => console.log(response.text()) )
      .catch( err => console.log(err));
    } else {
      console.log('No form element');
    }
    
}

</script>

<template>
  <div class="form-description">
    <h2>
      This is a test form with a <a href="https://make.com" target="_blank">make.com</a> as backend submit data manager.
    </h2>

    <p>
      How to use:
    </p>

    <ul>
      <li>Open this
        <a :href="GOOGLE_SHEET_URL" target="_blank">
          Google Sheet Link
        </a>
      </li>
      <li>Fill form and check your Google Sheet Link</li>
    </ul>
  </div>

  <!-- From -->
  <div class="form-container">
    <form ref="form" v-if="!isSubmitted" :action="WEBHOOK" method="post">
      <div class="fieldset">
        <label for="name">Name:</label>
        <input type="text" id="name" name="name" required>
      </div>
      <div class="fieldset">
        <label for="email">Email:</label>
        <input type="email" id="email" name="email" required>
      </div>
      <div class="fieldset">
        <label for="phone">phone:</label>
        <input type="tel" id="phone" name="phone" required>
      </div>
      <div class="fieldset">
        <label for="industry">Sector:</label>
        <select id="industry" name="industry">
          <option value="public">Public</option>
          <option value="private">Private</option>
        </select>
      </div>
      <div class="fieldset">
        <button type="submit" @click="handleSubmit">Send it!</button>
      </div>
    </form>

    <div v-else class="response-box">
      <p>
        Wait a second and check this <a :href="GOOGLE_SHEET_URL" target="_blank">Google Sheet Link</a> again with your submitted data.
      </p>

      <button @click="handleSubmit">Try it again.</button>
    
    </div>
  </div>

</template>

<style scoped>

.form-description {
  margin: 50px 0px;
}

.form-container {
  padding: 5% 50px;
  display: flex;
  flex-direction: column;
  align-content: center;
  align-items: center;
  border: 2px white solid;
}

h1, h2 {
  margin-bottom: 30px;
}
form {
  max-width: 280px;
}
.fieldset {
  margin: 12px;
  display: flex;
  flex-direction: column;
}

input, select {
  line-height: 2rem;
  height: 2rem;
}

button {
  height: 2rem;
  margin: 10px 0px;
}

.response-box {
  margin: 50px;
  border: 2px white solid;
  padding: 50px;
  text-align: center;
}


</style>
