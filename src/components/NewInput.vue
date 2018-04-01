<template>
 <div id="NewInput">
   <div class="outer-frame">

     <h1>Commercial Property - Add Field</h1>

     <!-- saved fields -->
     <p class="red">Saved Fields</p>
     {{ savedFields }}

     <br>
     <br>
     <br>
     <br>
     <br>

     <!-- new field, yet to be saved -->
     <p>Unsaved Fields</p>
     {{ newField }}
     <br>
     <br>

     <!-- first, the user must select a type -->

     <!-- input to search input types -->
     <p>Search Bar and Types</p>
     <input v-model="searchText">

      <div v-if="types">
        <ul>
          <li v-for="type in typeSearched" @click="selectType(type)">{{ type }} </li>
        </ul>
      </div>


     <p>New Input Inputs</p>
     <!-- inputs displayed only once type is selected -->
     <div v-if="newField.type">
       <label for="displayLabel">Display Label</label>
       <br>
       <input type="text" id="displayLabel" v-model="newField.displayLabel" @blur="generateRefName()">
       <br>
       <label for="defaultValue">Default Value</label>
       <br>
       <input type="text" id="defaultValue" v-model="newField.defaultValue">
       <br>
       <label for="pattern">Custom Validation</label>
       <br>
       <input type="text" id="pattern" v-model="newField.pattern" @blur="checkRegex()">
       <br>
       <label for="referenceName">Reference Name</label>
       <br>
       <input type="text" id="referenceName" v-model="newField.referenceName" @blur="removeSpaces()">

     <p>Tags</p>
       <!-- tags, conditionally rendered by input type -->
       <div v-if="newField.type == 'date'">
         <button v-for="(tags, group) in dateTagGroups" @click="toggleTagsInGroup(tags, group)">{{ group }}</button>
         <p v-for="(tag, group) in newField.tags">{{ tag.join(" ") }}</p>
       </div>

       <div v-if="newField.type == 'number'">
         <button v-for="(tags, group) in numberTagGroups" @click="toggleTagsInGroup(tags, group)">{{ group }}</button>
         <p v-for="(tag, group) in newField.tags">{{ tag.join(" ") }}</p>
       </div>

       <div v-if="newField.type == 'currency'">
         <button v-for="(tags, group) in currencyTagGroups" @click="toggleTagsInGroup(tags, group)">{{ group }}</button>
         <p v-for="(tag, group) in newField.tags">{{ tag.join(" ") }}</p>
       </div>

       <div v-if="newField.type == 'text'">
         <button v-for="(tags, group) in textTagGroups" @click="toggleTagsInGroup(tags, group)">{{ group }}</button>
         <p v-for="(tag, group) in newField.tags">{{ tag.join(" ") }}</p>
       </div>

       <div v-if="newField.type == 'select'">
         <button v-for="(tags, group) in selectTagGroups" @click="toggleTagsInGroup(tags, group)">{{ group }}</button>
         <p v-for="(tag, group) in newField.tags">{{ tag.join(" ") }}</p>
       </div>

       <div v-if="newField.type == 'vin'">
         <button v-for="(tags, group) in vinTagGroups" @click="toggleTagsInGroup(tags, group)">{{ group }}</button>
         <p v-for="(tag, group) in newField.tags">{{ tag.join(" ") }}</p>
       </div>
       <!-- end of tags -->


       <div v-if="newField.type">
         <!-- delete, save, or cancel changes -->
         <button @click="deleteNew">Delete</button>

         <button @click="cancelChanges">Cancel Changes</button>

         <button @click="saveNew">Save</button>
       </div>




     <p>Field Groups</p>

       <!-- field groups -->
       <ul>
         <li v-for="group in fieldGroups" @click="toggleAddToFieldGroup(group)">{{ group.title }}</li>
       </ul>

       <input type="text" v-if="showNewFieldGroup" v-model="newFieldGroup.title">

       <br>

       <button v-if="!showNewFieldGroup" @click="showNewFieldGroupInput()">Add a New Group</button>

       <div v-if="showNewFieldGroup">
         <button @click="saveNewFieldGroup">Save New Group</button>
         <button @click="cancelNewFieldGroup()">Cancel</button>
       </div>


     </div>



     <br>
     <br>
     <br>


     <p>Field Groups:</p>
     <!-- field groups -->
     {{ fieldGroups }}

     <br>
     <br>
     <br>

     <p>New Field Group</p>
     {{ newFieldGroup }}





     <br>
     <br>
     <br>
     <br>
     <br>


   </div>







 </div>
</template>

<script>
import Vue from 'vue'

export default {
  name: 'NewInput',
  data () {
    return {
      // different input types
      types: [
        'date',
        'number',
        'currency',
        'text',
        'select',
        'vin'
      ],
      // Empty search input
      searchText: '',
      // example tag groups for each input type
      dateTagGroups: {
        dateTag1: [
          'dateTag 1-1',
          'dateTag 1-2',
          'dateTag 1-3'
        ],
        dateTag2: [
          'dateTag 2-1',
          'dateTag 2-2',
          'dateTag 2-3'
        ],
        dateTag3: [
          'dateTag 3-1',
          'dateTag 3-2',
          'dateTag 3-3'
        ],
      },
      numberTagGroups: {
        numberTag1: [
          'numberTag 1-1',
          'numberTag 1-2',
          'numberTag 1-3'
        ],
        numberTag2: [
          'numberTag 2-1',
          'numberTag 2-2',
          'numberTag 2-3'
        ],
      },
      currencyTagGroups: {
        currencyTag1: [
          'currencyTag 1-1',
          'currencyTag 1-2',
          'currencyTag 1-3'
        ],
        currencyTag2: [
          'currencyTag 2-1',
          'currencyTag 2-2',
          'currencyTag 2-3'
        ],
      },
      textTagGroups: {
        textTag1: [
          'textTag 1-1',
          'textTag 1-2',
          'textTag 1-3'
        ],
        textTag2: [
          'textTag 2-1',
          'textTag 2-2',
          'textTag 2-3'
        ],
        textTag3: [
          'textTag 3-1',
          'textTag 3-2',
          'textTag 3-3'
        ],
      },
      selectTagGroups: {
        selectTag1: [
          'selectTag 1-1',
          'selectTag 1-2',
          'selectTag 1-3'
        ],
        selectTag2: [
          'selectTag 2-1',
          'selectTag 2-2',
          'selectTag 2-3'
        ],
      },
      vinTagGroups: {
        vinTag1: [
          'vinTag 1-1',
          'vinTag 1-2',
          'vinTag 1-3'
        ],
        vinTag2: [
          'vinTag 2-1',
          'vinTag 2-2',
          'vinTag 2-3'
        ],
      },
      // When the user hits 'save', the data fron the newField object is pushed here, newField gets reset
      savedFields: [

      ],
      // New field goes here. When saved or canceled the values are reset
      newField: {
        type: null,
        displayLabel: null,
        defaultValue: null,
        pattern: null,
        referenceName: null,
        tags: {},
        fieldGroups: {}
      },
      // Example field groups
      fieldGroups: {
        rentalVehiclePackage: {
          title: 'Rental Vehicles Coverage Package',
          // otherInputs: 7
        },
        group2: {
          title: 'Group 2',
          // otherInputs: 1
        },
      },
      newFieldGroup: {
        title: null,
        // otherInputs: 0
      },
      showNewFieldGroup: false
    }
  },


    methods: {
      // When the user selects a type, the value for 'type' is set to the selected type
      selectType (type) {
        this.newField = {
          type: type,
          displayLabel: null,
          defaultValue: null,
          pattern: null,
          referenceName: null,
          tags: {},
          fieldGroups: {}
        }
      },
      // Generates reference name on blur from display label input by removing spaces
      generateRefName () {
        this.newField.referenceName = this.newField.displayLabel.replace(/\s/g,'');
      },
      // Removes spaces in case the user decides to edit the reference name
      removeSpaces () {
        var str = this.newField.referenceName;

        if(str.indexOf(' ') >= 0){
          this.newField.referenceName = str.replace(/\s/g,'');
        }
      },
      // Checks if user input is valid Regex
      checkRegex () {
        var isValid = true;
        var userInput = this.newField.pattern;
        try {
          new RegExp(userInput);
        } catch(e) {
            isValid = false;
        }
        if(!isValid) {
          alert("Please enter a valid Regular Expression");
          this.newField.pattern = null;
        }
      },
      // Toggles adding tags in group to newField.tags
      toggleTagsInGroup (tags, group) {
        var name = group;

        if (this.newField.tags[name]) {
          Vue.delete(this.newField.tags, [name]);
        } else {
          this.newField.tags = Object.assign({}, this.newField.tags);
          this.newField.tags[name] = tags;
        }
      },
      // Resets newField's values back to null
      deleteNew () {
        this.newField = {
          type: null,
          displayLabel: null,
          defaultValue: null,
          pattern: null,
          referenceName: null,
          tags: {},
          fieldGroups: {}
        };
        alert('Input deleted!');
      },
      // Resets newField's values back to null
      cancelChanges () {
        this.newField = {
          type: null,
          displayLabel: null,
          defaultValue: null,
          pattern: null,
          referenceName: null,
          tags: {},
          fieldGroups: {}
        };
      },
      // Pushes newField to savedFields, sets newField's values to null
      saveNew () {
        if (this.newField.type
          && this.newField.displayLabel
          && this.newField.defaultValue
          && this.newField.pattern
          && this.newField.referenceName) {
            this.savedFields.push(this.newField);
            this.newField = {
              type: null,
              displayLabel: null,
              defaultValue: null,
              pattern: null,
              referenceName: null,
              tags: {},
              fieldGroups: {}
            };
            alert('input saved!');
            console.log(this.savedFields);
        } else {
          alert('Please fill out all fields');
        }
      },
      // Inputs related to field groups
      // Toggles adding input to field group
      toggleAddToFieldGroup (group) {
        var name = group.title.replace(/\s/g,'');

        if (this.newField.fieldGroups[name]) {
          Vue.delete(this.newField.fieldGroups, [name]);
        } else {
          this.newField.fieldGroups = Object.assign({}, this.newField.fieldGroups);
          this.newField.fieldGroups[name] = group;
        }
      },
      // Shows input to add a new field group
      showNewFieldGroupInput () {
        this.showNewFieldGroup = true;
      },
      // cancels changes, sets title back to null
      cancelNewFieldGroup () {
        this.newFieldGroup.title = null;
        this.showNewFieldGroup = false;
      },
      // pushes new, edited field group into the fieldGroups object,
      // sets newFieldGroup back to null
      saveNewFieldGroup () {
        var name = this.newFieldGroup.title.replace(/\s/g,'');
        var title = this.newFieldGroup.title;

        if (this.newFieldGroup.title) {
          this.fieldGroups = Object.assign({}, this.fieldGroups);
          this.fieldGroups[name] = {
            title: title,
            // otherInputs: 0
          }

          this.newFieldGroup = {
            title: null,
            // otherInputs: 0
          }

          this.showNewFieldGroup = false;
        } else {
          alert('please enter a title');
        }
      },
    },
    computed: {
      typeSearched () {
        var self = this;
        if( this.searchText == ''){
          return this.types;
        }
        return this.types.filter(function(type){
          return type.indexOf(self.searchText) >= 0;
        });
      }
    },
}
</script>

<style lang="scss">

  // Variables
  $outer-frame-background: #f9fbfb;
  $selected-type-background: #367f95;
  $field-type-background: #eef5f5;
  $tag-background: #eef5f5;
  $tag-borders: #d0e1e3;
  $save-button-background: #3d8da6;
  $input-borders: #bcd1d3;
  $field-groups-background: #f9fbfb;
  $delete-button-background: #b72f26;

  #NewInput {
    .outer-frame {
      background: $outer-frame-background;
      padding: 40px 60px;
    }
  }

</style>
