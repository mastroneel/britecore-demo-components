<template>
 <div id="NewInput">
   <div class="outer-frame">
     <h1>Commercial Property - Add Field</h1>
     <div class="inner">

       <nav class="site-navbar" v-if="newField.type">
         <input type="checkbox" id="toggle_menu" class="toggle-menu-checkbox" />
         <label for="toggle_menu" class="toggle-menu-label uppercase" id="toggle_menu">Show Field Groups</label>
         <input type="checkbox" id="toggle_overlay" class="toggle-overlay-checkbox" />
         <label for="toggle_menu" class="toggle-overlay-label uppercase"></label>
         <div class="site-menu">
           <div class="col-xs-12">
             <div class="field-groups field-groups-mobile">
               <h3>Field Groups</h3>

               <p>Choose a group for this input</p>

               <!-- field groups -->
               <ul>
                 <li v-for="group in fieldGroups" :class="{active:selectedFieldGroups.includes(group)}" @click="toggleAddToFieldGroup(group)">{{ group.title }}</li>
               </ul>

               <input type="text" v-if="showNewFieldGroup" v-model="newFieldGroup.title" placeholder="Enter name of new field group...">

               <button class="add-field-group" v-if="!showNewFieldGroup" @click="showNewFieldGroupInput()">Add a New Group</button>

               <div v-if="showNewFieldGroup" class="save-cancel-container">
                 <button class="save-button" @click="saveNewFieldGroup">Save New Group</button>
                 <button @click="cancelNewFieldGroup()">Cancel</button>
               </div>
             </div>
           </div>
         </div>
       </nav>


      <!-- desktop field types -->
       <div class="row field-types-desktop">
         <div class="col-xs-5 col-sm-4 col-md-3">
           <!-- container for field types section -->
           <div class="field-types-container">
             <div class="field-types-inner">
               <h2 class="field-types">Field Types</h2>
               <h4>Filter Types</h4>
               <!-- searchbar -->
               <input v-model="searchText">

               <!-- list of types -->
                <div v-if="types">
                  <ul>
                    <li v-for="type in typeSearched" @click="selectType(type)" :class="{selected:type.type == selectedType}">
                      <h2>
                      <i v-if="type.type == 'Text'" class="fa fa-font"></i>
                      <i v-if="type.type == 'Date'" class="fa fa-calendar"></i>
                      <i v-if="type.type == 'VIN'" class="fa fa-car"></i>
                      <i v-if="type.type == 'Number'" class="fa fa-hashtag"></i>
                      <i v-if="type.type == 'Currency'" class="fa fa-usd"></i>
                      <i v-if="type.type == 'Select'" class="fa fa-check-circle"></i>
                        {{ type.type }}
                      </h2>
                      <br>
                      <p>Definition</p>
                      <h3>{{ type.definition }}</h3>
                      <br>
                      <p>Default Display</p>
                      <h3>{{ type.defaultDisplay }}</h3>
                    </li>
                  </ul>
                </div>
              </div>
           </div>
         </div>
         <!-- before the user selects a type, this div is displayed -->
         <div class="placeholder-text col-xs-6 col-sm-8 col-md-9" v-if="!newField.type">
           <p>First, please select a type.</p>
         </div>
         <!-- inputs displayed only once type is selected -->
           <div class="col-xs-6 col-sm-8 col-md-6 inputs-container" v-if="newField.type">

             <h2>Field Details</h2>
             <div class="row">
               <div class="col-xs-12 col-sm-6">
                 <h4>Display Label</h4>
                 <input type="text" v-model="newField.displayLabel" @blur="generateRefName()">
                 <p>For display purposes, spaces allowed</p>
                 <h4 class="margin">Default Value</h4>
                 <input type="text" v-model="newField.defaultValue">
                 <h4 class="margin">Custom Validation</h4>
                 <input type="text" v-model="newField.pattern" @blur="checkRegex()">
                 <p>Any regex pattern can be used for custom input validation</p>
               </div>
               <div class="col-xs-12 col-sm-6">
                 <h4>Reference Name</h4>
                 <input type="text" v-model="newField.referenceName" @blur="removeSpaces()">
                 <p>Used to reference in calculations, no spaces allowed</p>
               </div>
             </div>
             <div class="row">
               <div class="col-xs-12">
                 <h3>Tags</h3>
               </div>
             </div>
             <div class="row tag-row">
               <!-- tag groups -->
               <div class="col-xs-6">
                 <h4>Tag Groups</h4>
                 <button v-if="newField.type == 'Date'" v-for="(tags, group) in dateTagGroups" :class="{active:selectedTags.includes(group)}" @click="toggleTagsInGroup(tags, group)">{{ group }}</button>
                 <button v-if="newField.type == 'Number'" v-for="(tags, group) in numberTagGroups" :class="{active:selectedTags.includes(group)}" @click="toggleTagsInGroup(tags, group)">{{ group }}</button>
                 <button v-if="newField.type == 'Currency'" v-for="(tags, group) in currencyTagGroups" :class="{active:selectedTags.includes(group)}" @click="toggleTagsInGroup(tags, group)">{{ group }}</button>
                 <button v-if="newField.type == 'Text'" v-for="(tags, group) in textTagGroups" :class="{active:selectedTags.includes(group)}" @click="toggleTagsInGroup(tags, group)">{{ group }}</button>
                 <button v-if="newField.type == 'Select'" v-for="(tags, group) in selectTagGroups" :class="{active:selectedTags.includes(group)}" @click="toggleTagsInGroup(tags, group)">{{ group }}</button>
                 <button v-if="newField.type == 'VIN'" v-for="(tags, group) in vinTagGroups" :class="{active:selectedTags.includes(group)}" @click="toggleTagsInGroup(tags, group)">{{ group }}</button>
               </div>
               <!-- tags from tag groups -->
               <div class="col-xs-6">
                 <h4>Tags</h4>
                 <ul>
                   <li v-for="(tag, group) in newField.tags">{{ tag.join(" ") }}</li>
                 </ul>
               </div>
             </div>
           </div>
           <!-- field groups -->
           <div class="col-sm-4 col-md-3" v-if="newField.type">
             <div class="field-groups field-groups-desktop">
               <h3>Field Groups</h3>

               <p>Choose a group for this input</p>

               <!-- field groups -->
               <ul>
                 <li v-for="group in fieldGroups" :class="{active:selectedFieldGroups.includes(group)}" @click="toggleAddToFieldGroup(group)">{{ group.title }}</li>
               </ul>

               <input type="text" v-if="showNewFieldGroup" v-model="newFieldGroup.title" placeholder="Enter name of new field group...">

               <button class="add-field-group" v-if="!showNewFieldGroup" @click="showNewFieldGroupInput()">Add a New Group</button>

               <div v-if="showNewFieldGroup" class="save-cancel-container">
                 <button class="save-button" @click="saveNewFieldGroup">Save New Group</button>
                 <button @click="cancelNewFieldGroup()">Cancel</button>
               </div>
             </div>
           </div>

       </div>
       <!-- end row -->















     <!-- saved fields -->
     <!-- <p class="red">Saved Fields</p>
     {{ savedFields }}

     <br>
     <br> -->

     <!-- new field, yet to be saved -->
     <!-- <p>Unsaved Fields</p>
     {{ newField }}
     <br>
     <br> -->

     <!-- first, the user must select a type -->

     <!-- input to search input types -->







     <br>
     <br>
     <br>


     <!-- <p>Field Groups:</p> -->
     <!-- field groups -->
     <!-- {{ fieldGroups }} -->

     <br>
     <br>
     <br>

     <!-- <p>New Field Group</p> -->
     <!-- {{ newFieldGroup }} -->



   </div>

   <!-- save, cancel, delete buttons in the outer container -->
   <div v-if="newField.type" class="button-container">
     <button class="save-button" @click="saveNew">Save Changes</button>

     <button class="delete-button" @click="deleteNew">Delete Input</button>

     <button class="cancel-button" @click="cancelChanges">Cancel Changes</button>

   </div>



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
      // types: [
      //   'date',
      //   'number',
      //   'currency',
      //   'text',
      //   'select',
      //   'vin'
      // ],
      selectedTags: [],
      selectedFieldGroups: [],
      types: [
        {
          type: 'Text',
          definition: 'String of text',
          defaultDisplay: 'Free form text input'
        },
        {
          type: 'Date',
          definition: 'Standard ISO format date',
          defaultDisplay: 'Datepicker, with configurable format'
        },
        {
          type: 'VIN',
          definition: 'Vehicle Identification Number',
          defaultDisplay: 'Free form text input'
        },
        {
          type: 'Number',
          definition: 'Number',
          defaultDisplay: 'Number input'
        },
        {
          type: 'Currency',
          definition: 'Currency',
          defaultDisplay: 'Currency input'
        },
        {
          type: 'Select',
          definition: 'Select',
          defaultDisplay: 'Select menu'
        }
      ],
      selectedType: undefined,
      // Empty search input
      searchText: '',
      // Tag is selected?


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
          type: type.type,
          displayLabel: null,
          defaultValue: null,
          pattern: null,
          referenceName: null,
          tags: {},
          fieldGroups: {}
        }
        this.selectedType = type.type;
        this.selectedTags = [];
        this.selectedFieldGroups = [];
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

        this.selectedTags.includes(group) ? this.selectedTags.splice(this.selectedTags.indexOf(group), 1) : this.selectedTags.push(group);

        if (this.newField.tags[name]) {
          Vue.delete(this.newField.tags, [name]);
          this.tagIsActive = false;
        } else {
          this.newField.tags = Object.assign({}, this.newField.tags);
          this.newField.tags[name] = tags;
          this.tagIsActive = true;
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

        this.selectedFieldGroups.includes(group) ? this.selectedFieldGroups.splice(this.selectedFieldGroups.indexOf(group), 1) : this.selectedFieldGroups.push(group)

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
          return type.type.toLowerCase().indexOf(self.searchText.toLowerCase()) >= 0;
        });
      }
    },
}
</script>

<style lang="scss">

  // Variables
  $main-font-color: #3d3d3d;
  $secondary-font-color: #41676c;
  $outer-frame-background: #f9fbfb;
  $selected-type-background: #367f95;
  $field-type-background: #eef5f5;
  $tag-background: #eef5f5;
  $tag-border-color: #d0e1e3;
  $save-button-background: #3d8da6;
  $input-border-color: #bcd1d3;
  $field-groups-background: #f9fbfb;
  $delete-button-background: #b72f26;
  $white: #fff;
  $li-border-color: #d8e6e7;
  $icon-color: #1d4a53;
  $selected-type-text: #f7fbfc;
  $selected-type-icon: #b3dbe1;

  #NewInput {
    // Outer frame styles
    .outer-frame {
      background: $outer-frame-background;
      padding: 20px 40px;
      h1 {
        font-size: 30px;
        margin-bottom: 20px;
      }

      // save, cancel, delete button styles
      .button-container {
        margin: 20px 0;
        padding-bottom: 40px;
        button {
          height: 35px;
          padding: 0 25px;
          border-radius: 3px;
          font-family: 'Proxima Nova', sans-serif;
          font-weight: bold;
          font-size: 13px;
        }
        .save-button {
          background: $save-button-background;
          border: 2px solid $save-button-background;
          color: $white;
          float: left;
        }
        .cancel-button, .delete-button {
          float: right;
        }
        .cancel-button {
          margin-right: 30px;
          border: 2px solid $li-border-color;
        }
        .delete-button {
          border: 2px solid $delete-button-background;
          background: $delete-button-background;
          color: $white;
        }
      }

      // Inner container styles
      .inner {
        border: 2px solid $input-border-color;
        border-radius: 5px;
        background: $white;
        height: 75vh;
        min-height: 540px;
        input {
          border: 2px solid $input-border-color;
          border-radius: 5px;
          height: 30px;
        }
        // field types styles
        .field-types-container {
          background-color: $field-type-background;
          float: left;
          height: 75vh;
          min-height: 540px;
          overflow-y: scroll;
          .field-types-inner {
            padding: 20px 25px 20px 20px;
          }
          .field-types {
            margin-bottom: 10px;
            font-size: 20px;
          }
          h4 {
            margin-bottom: 10px;
            font-size: 14px;
          }
          input {
            width: 100%;
          }
          ul {
            list-style-type: none;
            margin: 0;
            padding: 0;
            li {
              background: $white;
              margin: 12px 0;
              padding: 15px;
              border: 2px solid $li-border-color;
              border-radius: 5px;
              line-height: 0.9;
              i {
                color: $icon-color;
                margin-right: 3px;
              }
              h2 {
                font-size: 16px;
              }
              h3 {
                font-size: 14px;
              }
              p {
                font-size: 12px;
              }
            }
            li.selected {
              background: $selected-type-background;
              border-color: $selected-type-background;
              h2, h3, {
                color: $selected-type-text;
              }
              p, i {
                color: $selected-type-icon;
              }
            }
          }
        }
        .placeholder-text {
          p {
            text-align: center;
            font-size: 18px;
            margin-top: 20%;
          }
        }
        // edit inputs container styles
        .inputs-container {
          padding-top: 20px;
          // please select a type text
          input {
            width: 90%;
          }
          h2 {
            font-size: 20px;
            margin-bottom: 10px;
          }
          h4 {
            font-size: 14px;
            margin: 5px 0 5px 0;
          }
          .margin {
            margin-top: 40px;
          }
          p {
            font-size: 12px;
            margin-top: 5px;
          }
        }
      }

      // tag styles
      .tag-row {
        h3 {
          margin: 0 0 15px 0;
        }
        h4 {
          margin: 10px 0;
        }
        // tag group buttons
        button {
          background: $tag-background;
          color: $secondary-font-color;
          border: 2px solid $tag-border-color;
          border-radius: 5px;
          font-family: 'Proxima Nova', sans-serif;
          font-weight: bold;
          font-size: 12px;
          padding: 8px 15px;
          margin: 3px 5px;
          &:focus {
            outline: none;
          }
        }
        button.active {
          color: $tag-background;
          background-color: $secondary-font-color;
          border-color: $selected-type-background;
        }
        // tag ul styles
        ul {
          margin: 0;
          padding: 0;
          list-style: none;
          li {
            font-size: 12px;
            line-height: 1.5;
            font-weight: bold;
            color: $secondary-font-color;
          }
        }
      }


      // field groups styles
      .field-groups {
        background: $field-groups-background;
        border: 2px solid $tag-border-color;
        border-radius: 5px;
        width: 80%;
        padding: 5%;
        margin-top: 20px;
        min-height: 430px;
        position: relative;
        input {
          width: 98%;
          padding-left: 5px;
          font-family: 'Proxima Nova', sans-serif;
        }
        ul {
          list-style-type: none;
          margin: 0 0 15px 0;
          padding: 0;
          height: 250px;
          overflow-x: scroll;
          li {
            font-size: 13px;
            font-weight: bold;
            color: $main-font-color;
            background: $white;
            border: 2px solid $tag-border-color;
            border-radius: 5px;
            padding: 10px 15px;
            margin: 15px 0;
          }
          li.active {
            color: $white;
            background: $secondary-font-color;
          }
        }
        button {
          border: 2px solid $li-border-color;
          background: $white;
          height: 35px;
          border-radius: 3px;
          font-family: 'Proxima Nova', sans-serif;
          font-weight: bold;
          font-size: 12px;
        }
        .add-field-group {
          width: 90%;
          position: absolute;
          bottom: 10px;
        }
        .save-cancel-container {
          display: block;
          position: absolute;
          width: 100%;
          bottom: 10px;
          button {
            width: 45%;
          }
          .save-button {
            color: $white;
            background-color: $selected-type-background;
            border-color: $selected-type-background;
          }
        }
      }
    }
  }

  // *************
  // Media queries
  // *************

  @media (min-width: 1500px) and (min-height: 1000px) {
    .outer-frame {
      .inner {
        .field-types-container {
          width: 90%;
        }
        .inputs-container {
           input {
             height: 40px !important;
           }
           h2 {
             font-size: 30px !important;
             margin-top: 20px !important;
           }
           h3 {
             font-size: 30px !important;
             margin-top: 30px !important;
           }
           h4 {
             font-size: 25px !important;
             margin: 10px 0 !important;
           }
           p {
             font-size: 16px !important;
           }
           .margin {
             margin-top: 50px !important;
           }
           .tag-row {
             button {
               font-size: 16px !important;
             }
             ul {
               li {
                 font-size: 16px !important;
               }
             }
           }
        }
      }
    }
  }

  @media (min-width: 1900px) and (min-height: 1100px) {
    .outer-frame {
      .inner {
        .field-types-container {
          width: 90%;
        }
        .inputs-container {
           input {
             height: 45px !important;
           }
           h2 {
             font-size: 32px !important;
             margin-top: 20px !important;
           }
           h3 {
             font-size: 32px !important;
             margin-top: 30px !important;
           }
           h4 {
             font-size: 28px !important;
             margin: 10px 0 !important;
           }
           p {
             font-size: 18px !important;
           }
           .margin {
             margin-top: 50px !important;
           }
           .tag-row {
             button {
               font-size: 18px !important;
             }
             ul {
               li {
                 font-size: 18px !important;
               }
             }
           }
        }
      }
    }
  }

  @media (max-width: 1090px) {
    .outer-frame {
      padding: 0 !important;
      h1 {
        display: none;
      }
      .save-button {
        margin-left: 20px;
      }
      .delete-button {
        margin-right: 20px;
      }
      .inner {
        border: none !important;
        border-bottom: 2px solid $input-border-color !important;
      }
    }
  }

  @media (min-width: 1024px) {
    .outer-frame {
      .site-navbar {
        display: none;
      }
      .inner {
        .field-groups-mobile {
          display: none;
        }
      }
    }
  }

  @media (max-width: 1023px) {
    .outer-frame {
      .inner {
        .field-groups-desktop {
          display: none;
        }
        .field-groups-mobile {
          display: block;
          .save-cancel-container {
            button {
              width: 38% !important;
              font-size: 10px !important;
            }
          }
        }
      }
    }
  }

  @media (min-width: 768px) {
    .outer-frame {
      .inner {
        .field-types-mobile {
          display: none;
        }
        .placeholder-text {
          display: none;
        }
      }
    }
  }

  @media (max-width: 767px) {
    .outer-frame {
      button {
        padding: 5px !important;
        font-size: 10px !important;
      }
      .inner {
        min-height: 630px !important;
        .field-types-container {
          min-height: 630px !important;
        }
        .inputs-container {
          margin-left: 20px;
          .margin {
            margin-top: 20px !important;
          }
        }
      }
    }
  }

  @media (max-width: 515px) {
    .outer-frame {
      .inner {
        min-height: 690px !important;
        .field-types-container {
          min-height: 690px !important;
          .field-types-inner {
            padding: 10px !important;
            ul {
              li {
                i {
                  font-size: 10px;
                }
                h2 {
                  font-size: 14px !important;
                }
                h3 {
                  font-size: 12px !important;
                }
                p {
                  font-size: 10px !important;
                }
              }
            }
          }
        }
      }
    }
  }
  //
  // @media (max-width: 470px) {
  //   .outer-frame {
  //     .inner {
  //       .field-types-desktop {
  //         .field-types-container {
  //           .field-types-inner {
  //             padding: 10px !important;
  //           }
  //         }
  //       }
  //     }
  //   }
  // }



  // ******************************
  // Mobile field group menu styles
  // ******************************

  // mobile menu variables
  $toggle-stripe-2: 10px;
  $toggle-stripe-3: 20px;

  $navbar-background: $field-type-background;;
  $navbar-color: $secondary-font-color;

  $transition: .2s ease-in-out;

  $menu-width: 300px;
  $menu-shadow: 4px 0 16px -4px rgba(0, 0, 0, .2);

  %transition {
   -webkit-transition: $transition;
      -moz-transition: $transition;
           transition: $transition;
  }

  // field group mobile menu styles
  .site-navbar {
    font-size: 14px;
    height: 60px;
    left: 20px;
    position: fixed;
    right: 20px;
    top: 20px;
    float: right;
    z-index: 11;
    label {
      padding-left: 45px;
      padding-top: 4px;
      font-size: 16px;
      color: $secondary-font-color;
    }
  }

  // menu styles
  .site-menu {
    @extend %transition;
    background: $field-type-background;
    bottom: 0;
    font-size: 14px;
    height: 100%;
    left: -$menu-width;
    list-style: none;
    margin: 0;
    padding: 0;
    padding-top: 40px;
    position: fixed;
    top: 0;
    width: $menu-width;
  }

  // hides default checkbox input
  .toggle-overlay-checkbox {
    display: none;
  }

  // page overlay styles
  .toggle-overlay-label {
    @extend %transition;
    bottom: 0;
    left: 0;
    opacity: 0;
    pointer-events: none;
    position: fixed;
    right: 0;
    top: 0;
    z-index: -1;
  }
  .toggle-menu-checkbox:checked ~ .toggle-overlay-label {
    background: rgba(0, 0, 0, .5);
    opacity: 1;
    pointer-events: auto;
    z-index: 1;
  }

  // menu toggle
  .toggle-menu-checkbox,
  .toggle-menu-label {
    display: none;
  }
  .toggle-menu-checkbox:checked ~ .site-menu {
    left: 0;
    z-index: 2;
            box-shadow: $menu-shadow;
       -moz-box-shadow: $menu-shadow;
    -webkit-box-shadow: $menu-shadow;
  }
  .toggle-overlay-checkbox:checked ~ .site-menu {
    left: -$menu-width;
  }
  .toggle-menu-label {
    cursor: pointer;
    display: block;
    padding: 0 0 0 20px;
    position: absolute;
    left: 0px;
    top: 0px;
    user-select: none;
    -webkit-touch-callout: none;
    -webkit-user-select: none;
    &:before {
      background: $navbar-color;
      content: "";
      height: 4px;
      position: absolute;
      left: 0;
      top: 1px;
      width: 2.2em;
      box-shadow: 0 $toggle-stripe-2 0 0 $navbar-color, 0 $toggle-stripe-3 0 0 $navbar-color;
      -moz-box-shadow: 0 $toggle-stripe-2 0 0 $navbar-color, 0 $toggle-stripe-3 0 0 $navbar-color;
      -webkit-box-shadow: 0 $toggle-stripe-2 0 0 $navbar-color, 0 $toggle-stripe-3 0 0 $navbar-color;

    }
    &:after {
      color: $navbar-color;
      content: "";
      display: inline-block;
      line-height: 1;
      margin-top: -9px;
      padding-right: 1.4em;
      text-align: center;
      text-transform: uppercase;
      vertical-align: middle;
      border-radius: 2px 2px 2px 2px;
    }
  }


























</style>
