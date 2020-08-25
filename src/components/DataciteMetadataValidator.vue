<template>

<div>





  <b-container>
    <b-row>
      <div class="col-sm-4"></div>
      <div class="col-sm-4">

        <b-form-group id="fieldset-1" description="Let us know your name." label="Enter your name" label-for="input-1"
          >
          <b-form-select v-model="version" :options="versionList"></b-form-select>
          <div class="mt-3">Selected: <strong>{{ version }}</strong></div>

          <b-form-select v-model="schema" :options="schemaList"></b-form-select>
          <div class="mt-3">Selected: <strong>{{ schema }}</strong></div>
                <b-button @click="check" variant="primary">Check</b-button>

        </b-form-group>
      </div>
      <div class="col-sm-4">
          <b-form-textarea
            id="textarea"
            v-model="metadata"
            placeholder="Enter something..."
            rows="3"
            max-rows="6"
          ></b-form-textarea>

          <p>{{ validation }}</p>
      </div>
      <div class="col-sm-4"></div>
    </b-row>
    <b-row></b-row>
  </b-container>
</div>


</template>

<script>

import Ajv from 'ajv';
import xsdValidator from 'xsd-schema-validator';
import jsond from '../assets/jsonschema.json';
import grunt from 'grunt-xml-and-xsd-validator';

export default {
  name: 'DataciteMetadataValidator',
  props: {
  },
  data: function() { 
    return {
        metadata: "",
        validation: "",
        schema: "json",
        version: "4.2",
        versionList: ["4.2","4.3"],
        schemaList: ["json","xml"],
      }
    },
  computed:{
      statusClass(){
        return "color-dot " + this.indicator
      }
    },
  methods:{
    validateJson(){
      // eslint-disable-next-line no-console
      console.log("DFfdsdfsdfsfds");
      var ajv = new Ajv(); // options can be passed, e.g. {allErrors: true}
      var validate = ajv.compile(jsond);
      var valid = validate(this.metadata);
      if (!valid) this.validation = validate.errors;
    },
    validateXml(){
      // eslint-disable-next-line no-console
      console.log("222222");
      // xsdValidator.validateXML(this.metadata, '../assets/xml/42.xsd', function(err, result) {
      //   if (err) {
      //     // this.validation = err;
      //     throw err;
      //   }
      
      //   result.valid; // true
      // });
      grunt.loadNpmTasks('grunt-xml-and-xsd-validator');
      grunt.initConfig({
        xml_validator: {
          your_target: {
            src: [ this.metadata ],
            options: {
              xsd: '../assets/xml/42.xsd'
            }
          },
        },
      });
    },
    check(evt){
      evt.preventDefault()
      switch(this.schema) {
        case "xml":
          this.validateXml()
          break;
        default:
          this.validateJson()
      }
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url('https://fonts.googleapis.com/css?family=Raleway:400,600,400italic,600italic');
/* @import url('https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css'); */
/* @import url('https://maxcdn.bootstrapcdn.com/font-awesome/4.6.1/css/font-awesome.min.css'); */
@import url('https://assets.datacite.org/stylesheets/datacite.css');
</style>

