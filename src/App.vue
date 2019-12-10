<template>
  <div id="activityApp">
    <app-header @filterSelected="setFilter"></app-header>
    <section class="container">
      <div class="columns">
        <div class="column is-3">
          <!--  -->

          <activity-create :categories="categories"></activity-create>

          <!--  -->
        </div>
        <div class="column is-9">
          <div class="box content">
            <div v-if="isFatching" style="color:red">Loading...</div>
            <div v-else>
              <div v-if="isDateLoader">
                <activity-item
                  v-for="activity in filteredActivities"
                  :key="activity.id"
                  :categories="categories"
                  :activity="activity"
                ></activity-item>
              </div>
            </div>
            <div v-if="error" style="color:red">{{ error }}</div>
            <div v-else>
              <div class="activity-length">Currently {{ activityLength }} Activities</div>
              <div class="activity-motivation">{{ activityMotivation }}</div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import Vue from "vue";
import fakeApi from "./lib/fakeApi";
import store from "./store";
import ActivityItem from "./components/ActivityItem.vue";
import ActivityCreate from "./components/ActivityCreate.vue";
import Header from "./components/Header.vue";

export default {
  components: {
    ActivityItem,
    ActivityCreate,
    appHeader: Header
  },
  data() {
    const {
      state: { activities, categories }
    } = store;
    return {
      error: null,
      isFatching: false,
      user: {},
      activities,
      categories,
      filter: "all"
    };
  },
  computed: {
    filteredActivities() {
      let condition;
      if (this.filter === "all") {
        return this.activities;
      }
      if (this.filter === "inprogress") {
        condition = value => value > 0 && value < 100;
      } else if (this.filter === "finished") {
        condition = value => value === 100;
      } else {
        condition = value => value === 0;
      }
      return Object.values(this.activities).filter(activity =>
        condition(activity.progress)
      );
    },

    activityLength() {
      return Object.keys(this.activities).length;
    },
    activityMotivation() {
      if (this.activityLength && this.activityLength < 5) {
        return "Nice to see some activities";
      } else if (this.activityLength >= 5) {
        return "So many activity, good job";
      } else {
        return "Empty activity, So sad";
      }
    },

    activitiesLength() {
      return Object.keys(this.activities).length;
    },
    categoriesLength() {
      return Object.keys(this.categories).length;
    },

    isDateLoader() {
      return this.categoriesLength && this.activitiesLength;
    }
  },
  created() {
    //fakeApi.fillDB()

    this.isFatching = true;
    store
      .fatchActivity()
      .then(activities => {
        this.isFatching = false;
      })
      .catch(err => {
        this.error = err;
        this.isFatching = false;
      });
    this.user = store.fetchUser();
    store.fetchCategories().then(categories => {});
  },
  methods: {
    setFilter(filterOption) {
      this.filter = filterOption;
    }
  }
};
</script>

<style>
#activityApp {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

html,
body {
  font-family: "Open Sans", serif;
  background: #F2F6FA;
}
footer {
  background-color: #F2F6FA !important;
}
.fetching {
  border: 2px solid red "
}
.has-err {
   border: 2px solid red";
}
.activity-length {
  display: inline-block;
}
.activity-motivation {
  float: right;
}
.topNav {
  border-top: 5px solid #3498db;
}
.topNav .container {
  border-bottom: 1px solid #e6eaee;
}
.container .columns {
  margin: 3rem 0;
}
.navbar-menu .navbar-item {
  padding: 0 2rem;
}
aside.menu {
  padding-top: 3rem;
}
aside.menu .menu-list {
  line-height: 1.5;
}
aside.menu .menu-label {
  padding-left: 10px;
  font-weight: 700;
}
.button.is-primary.is-alt {
  background: #00c6ff;
  background: -webkit-linear-gradient(to bottom, #0072ff, #00c6ff);
  background: linear-gradient(to bottom, #0072ff, #00c6ff);
  font-weight: 700;
  font-size: 14px;
  height: 3rem;
  line-height: 2.8;
}
.media-left img {
  border-radius: 50%;
}
.media-content p {
  font-size: 14px;
  line-height: 2.3;
  font-weight: 700;
  color: #8f99a3;
}
article.post {
  margin: 1rem;
  padding-bottom: 1rem;
  border-bottom: 1px solid #e6eaee;
}
article.post:last-child {
  padding-bottom: 0;
  border-bottom: none;
}
.menu-list li {
  padding: 5px;
}
.navbar-brand > h1 {
  font-size: 31px;
  padding: 20px;
}
</style>
