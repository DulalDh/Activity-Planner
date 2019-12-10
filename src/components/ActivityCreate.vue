<template>
  <div>
    <a
      v-if="!isFormDisplayed"
      class="button is-primary is-block is-alt is-large"
      href="#"
      @click="toggleFormDisplayed"
    >New Activity</a>
    <div v-if="isFormDisplayed" class="create-form">

    <a class="button is-default is-block"><label class="label">Create Activity</label></a>
      <br>
      <form>
        <div class="field">
          <label class="label">Title</label>
          <div class="control">
            <input v-model="newActivity.title" class="input" type="text" placeholder="Read a Book" />
          </div>
        </div>
        <div class="field">
          <label class="label">Notes</label>
          <div class="control">
            <textarea v-model="newActivity.notes" class="textarea" placeholder="Write some notes" />
          </div>
        </div>

        <div class="field">
          <label class="label">Category</label>
          <div class="control">
            <select v-model="newActivity.category">
              <option disabled value>Select category</option>
              <option v-for="category in categories" :key="category.id" :value="category.id">{{ category.text }}</option>
            </select>
          </div>
        </div>

        <div class="field is-grouped">
          <div class="control">
            <button :disabled="!isFormValid" class="button is-link" @click.prevent="createActivity">
              <br />Create Activity
            </button>
          </div>
          <div style="float: right" class="control">
            <button class="button is-text" @click="toggleFormDisplayed">Cancel</button>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import store from '../store/index'
export default {
  props: {
    categories: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      isFormDisplayed: false,
        newActivity: {
        title: "",
        notes: "",
        category: ""
      },
    };
  },
  computed: {
    isFormValid() {
      return this.newActivity.title && this.newActivity.notes && this.newActivity.category;
    }
  },
  methods: {
    toggleFormDisplayed() {
      this.isFormDisplayed = !this.isFormDisplayed;
    },
    clearActivity(){
        this.newActivity.title = "",
        this.newActivity.notes = "",
        this.newActivity.category = ""
    },
    createActivity() { 
        store.createActivity({...this.newActivity})
        .then(activity => {
          this.clearActivity();
          this.toggleFormDisplayed(); 
        })
  
    }
  }
};
</script>