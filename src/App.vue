<template>
  <div id="app">
    <h1>V-Card Generator</h1>

    <form @submit.prevent="downloadVCard">

      <!-- For each data -->
      <div class="formField" v-for="singleData in vCardData" :key="singleData.key">

        <!-- When children exists -->
        <template v-if="singleData.parentName">

          <!-- For each children -->
          <div class="formField--children" v-for="singleChildren in singleData.children" :key="singleChildren.key">
            <label :for="singleChildren.property">{{singleChildren.title}}:</label> 
            <br>
            <input type="text" v-model="singleChildren.content" :id="singleChildren.property">
          </div>

        </template>

        <!-- When it is an single entry -->
        <template v-else>
          <label :for="singleData.property">{{singleData.title}}:</label> 
          <br>
          <input type="text" v-model="singleData.content" :id="singleData.property">
        </template>

      </div>

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
      vCardData: [
        {
          title: 'Vorname',
          property: 'firstName',
          content: ''
        },
        {
          title: 'Nachname',
          property: 'lastName',
          content: ''
        },
        {
          title: 'Titel',
          property: 'title',
          content: ''
        },
        {
          title: 'E-Mail (Geschäftlich)',
          property: 'workEmail',
          content: ''
        },
        {
          title: 'Telefon (Geschäftlich)',
          property: 'workPhone',
          content: ''
        },
        {
          title: 'Fax (Geschäftlich)',
          property: 'workFax',
          content: ''
        },
        {
          title: 'Firma',
          property: 'organization',
          content: ''
        },
        {
          parentName: 'homeAddress',
          children: [
            {
              title: 'Straße (Privat)',
              property: 'street',
              content: ''
            },
            {
              title: 'PLZ (Privat)',
              property: 'postalCode',
              content: ''
            },
            {
              title: 'Stadt (Privat)',
              property: 'city',
              content: ''
            },
            {
              title: 'Land (Privat)',
              property: 'countryRegion',
              content: ''
            }   
          ]
        },  
        {
          parentName: 'workAddress',
          children: [
            {
              title: 'Straße (Firma)',
              property: 'street',
              content: ''
            },
            {
              title: 'PLZ (Firma)',
              property: 'postalCode',
              content: ''
            },
            {
              title: 'Stadt (Firma)',
              property: 'city',
              content: ''
            },
            {
              title: 'Land (Firma)',
              property: 'countryRegion',
              content: ''
            }   
          ]
        }   
      ]
    }
  },
  computed: {},
  methods: {
    downloadVCard: function(){
      // Set data to vCard
      this.vCardData.forEach(singleData => {
        // When data contains children
        if(singleData.parentName){
          singleData.children.forEach(singleChildren => {
            vCard[singleData.parentName][singleChildren.property] = singleChildren.content;
          })
        } 
        // When it is an single entry
        else {
          vCard[singleData.property] = singleData.content;
        }
      });

      // Create file name
      let firstName = this.vCardData.find(element => element.property == 'firstName');
      let lastName = this.vCardData.find(element => element.property == 'lastName');
      let fileName = filenamify(firstName.content + '-' + lastName.content) + '.vcf';

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
