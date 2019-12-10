<template>
  <div>
    <nav class="navbar is-white topNav">
      <div class="container">
        <div class="navbar-brand">
          <h1>{{ fullAppName }}</h1>
        </div>
      </div>
    </nav>
    <nav class="navbar is-white">
      <div class="container">
        <div class="navbar-menu">
          <div class="navbar-start">
            <a
              :class="{'is-active': filterOption === 'all'}"
              class="navbar-item"
              @click="emitFilter('all')"
            >All</a>
            <a
              :class="{'is-active': filterOption === 'inprogress'}"
              class="navbar-item"
              @click="emitFilter('inprogress')"
            >In Progress</a>
            <a
              :class="{'is-active': filterOption === 'finished'}"
              class="navbar-item"
              @click="emitFilter('finished')"
            >Finished</a>
            <a
              :class="{'is-active': filterOption === 'notstarted'}"
              class="navbar-item"
              @click="emitFilter('notstarted')"
            >Not Started</a>
          </div>
        </div>
      </div>
    </nav>
  </div>
</template>

<script>
export default {
  data() {
    return {
      creator: "Dulal Hossin",
      appName: "Activity Planner",
      filterOption: "all"
    };
  },
  computed: {
    fullAppName() {
      return this.appName + " by " + this.creator;
    }
  },
  methods: {
    emitFilter(filterOption) {
      this.filterOption = filterOption;
      this.$emit("filterSelected", filterOption);
    }
  }
};
</script>

<style>
</style>