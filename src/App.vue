<template>
  <div id="app">
    <h1>V-Card Generator</h1>

    <form @submit.prevent="downloadVCard">

      <label for="firstName">Vorname:</label> 
      <br>
      <input type="text" v-model="firstName" id="firstName">

      <br>

      <label for="lastName">Nachname:</label> 
      <br>
      <input type="text" v-model="lastName" id="lastName">

      <br>

      <label for="role">Berufsbezeichnung:</label> 
      <br>
      <input type="text" v-model="role" id="role">

      <br>

      <label for="organization">Firma:</label> 
      <br>
      <input type="text" v-model="organization" id="organization">

      <br>

      <label for="workPhone">Telefon-Nummer (Geschäftlich):</label> 
      <br>
      <input type="text" v-model="workPhone" id="workPhone">

      <br>

      <label for="workFax">Fax-Nummer (Geschäftlich):</label> 
      <br>
      <input type="text" v-model="workFax" id="workFax">

      <br>

      <label for="workEmail">E-Mail (Geschäftlich):</label> 
      <br>
      <input type="text" v-model="workEmail" id="workEmail">

      <br>

      <button type="submit">vCard erstellen und downloaden</button>

    </form>
  </div>
</template>

<script>
// Import Dependencies
let vCard = require('vcards-js');
const filenamify = require('filenamify');

// Create a new vCard
vCard = vCard();

export default {
  name: 'app',
  components: {

  },
  data: function(){
    return {
      firstName: '',
      lastName: '',
      organization: '',
      workPhone: '',
      role: '',
      workFax: '',
      workEmail: ''
    }
  },
  computed: {},
  methods: {
    downloadVCard: function(){
      // Set data to vCard
      vCard.firstName = this.firstName;
      vCard.lastName = this.lastName;
      vCard.organization = this.organization;
      vCard.workPhone = this.workPhone;
      vCard.role = this.role;
      vCard.workFax = this.workFax;
      vCard.workEmail = this.workEmail;

      // Create file name
      let fileName = filenamify(this.firstName + '-' + this.lastName) + '.vcf';

      // Download vCard client-side with virtual link
      let dataString = 'data:text/vcard;charset=utf-8,' + vCard.getFormattedString();
      let downloadElement = document.createElement('a');
      downloadElement.setAttribute('href', dataString);
      downloadElement.setAttribute('download', fileName);
      document.body.appendChild(downloadElement);
      downloadElement.click();
      downloadElement.remove();
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin: 60px;
}
</style>
