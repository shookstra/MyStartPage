<template>
  <div>
    <div class="modal-background" v-if="addColumnToggled"></div>
    <b-button
      @click="settingsToggled = !settingsToggled"
      class="settings-button"
      type="is-primary"
      size="is-large"
      icon-left="cog"
    />
    <p>{{columnName}}</p>
    <!-- Area where settings options are located -->
    <div v-if="settingsToggled" class="settings-area">
      <p class="title is-size-4 has-text-light has-text-centered">Settings</p>
      <div class="field">
        <label for>Name</label>
        <b-input v-model="name"></b-input>
      </div>
      <div class="field">
        <b-switch v-model="twelveHour" passive-type="is-dark" type="is-primary">12-hour Clock</b-switch>
      </div>
      <div class="field">
        <b-switch
          v-model="lightMode"
          passive-type="is-dark"
          type="is-warning"
          true-value="Light Mode (WIP)"
          false-value="Dark Mode (WIP)"
        >{{ lightMode }}</b-switch>
      </div>
      <b-button
        @click="reloadPage"
        class="is-primary settings-save-button"
        size="is-medium"
        icon-left="content-save-settings"
      >Save</b-button>
    </div>

    <!-- Area where the add column modal is located -->
    <div v-if="addColumnToggled" class="add-column-menu">
      <b-button
        class="is-danger close-column-button"
        size="is-medium"
        icon-left="close"
        @click="addColumnToggled = !addColumnToggled"
      ></b-button>
      <p class="title is-size-4 has-text-light has-text-centered">Add New Column</p>
      <div class="field">
        <label for="columnName" class="is-size-5">Column Name</label>
        <b-input v-model="columnName"></b-input>
      </div>
      <b-button
        class="is-primary column-add-button"
        size="is-medium"
        icon-left="plus-circle"
      >Add Column</b-button>
    </div>

    <div class="main-container container">
      <p v-if="name === ''" class="is-size-1 has-text-white">Let's Get Started</p>
      <p v-if="name" class="is-size-1 has-text-white">Welcome, {{name}}</p>
      <p class="title has-text-white is-size-4">{{ time }}</p>

      <b-field v-on:keyup.enter.native="searchIconClick">
        <b-input
          placeholder="Duckduckgo Search..."
          type="search"
          icon-right="magnify"
          icon-right-clickable
          size="is-large"
          v-model="searchTerm"
          id="search-box"
          @icon-right-click="searchIconClick"
        ></b-input>
      </b-field>

      <div class="columns">
        <Column name="Social Media" :links="socialMediaLinks" />
        <Column name="Games" :links="gameLinks" />
        <Column name="Programming" :links="programmingLinks" />
        <Column name="Misc." :links="miscLinks" />
      </div>
      <b-button
        @click="addColumnToggled = !addColumnToggled"
        class="is-primary add-button"
        size="is-medium"
        icon-left="plus-circle"
      >Add Column</b-button>
    </div>
  </div>
</template>

<script>
import Column from "./Column.vue";

export default {
  name: "StartPage",
  data: function() {
    return {
      lightMode: false,
      twelveHour: false,
      searchTerm: "",
      time: "",
      name: "",
      columnName: "",
      settingsToggled: false,
      addColumnToggled: false,
      socialMediaLinks: [
        {
          id: 1,
          name: "Reddit (Old)",
          link: "https://old.reddit.com"
        },
        {
          id: 2,
          name: "Twitter",
          link: "https://www.twitter.com"
        },
        {
          id: 3,
          name: "Outlook",
          link:
            "https://www.microsoft.com/en-us/microsoft-365/outlook/email-and-calendar-software-microsoft-outlook"
        },
        {
          id: 4,
          name: "Gmail",
          link: "https://mail.google.com/mail/u/0/#inbox"
        },
        {
          id: 5,
          name: "YouTube",
          link: "https://www.youtube.com"
        }
      ],
      programmingLinks: [
        {
          id: 1,
          name: "Stack Overflow",
          link: "https://www.stackoverflow.com"
        },
        {
          id: 2,
          name: "MDN",
          link: "https://developer.mozilla.org/en-US/"
        }
      ],
      gameLinks: [
        {
          id: 1,
          name: "Steam",
          link: "https://store.steampowered.com/"
        },
        {
          id: 2,
          name: "IsThereAnyDeal",
          link: "https://isthereanydeal.com/"
        },
        {
          id: 3,
          name: "Chrono.gg",
          link: "https://www.chrono.gg/"
        },
        {
          id: 4,
          name: "Humble Bundle",
          link: "https://www.humblebundle.com/"
        },
        {
          id: 5,
          name: "ProtonDB",
          link: "https://www.protondb.com/"
        }
      ],
      miscLinks: [
        {
          id: 1,
          name: "Bandwidth",
          link: "http://bw.bloombb.net/"
        },
        {
          id: 2,
          name: "Bank",
          link: "https://www.pinnbank.com/"
        },
        {
          id: 3,
          name: "Discover",
          link: "https://www.discover.com/"
        }
      ]
    };
  },
  components: {
    Column
  },
  methods: {
    addColumn: function() {
      // alert("Add column");
    },
    searchIconClick: function() {
      if (this.searchTerm !== "") {
        window.location.assign(
          "https://www.duckduckgo.com?q=" + this.searchTerm
        );
      }
    },
    reloadPage: function() {
      location.reload();
    },
    // 24-hour time functions from
    // https://www.w3schools.com/js/tryit.asp?filename=tryjs_timing_clock
    startTime: function() {
      var today = new Date();
      var h = today.getHours();
      var m = today.getMinutes();
      var s = today.getSeconds();
      m = this.checkTime(m);
      s = this.checkTime(s);
      this.time = h + ":" + m + ":" + s;
      setTimeout(this.startTime, 500);
    },
    checkTime: function(i) {
      if (i < 10) {
        i = "0" + i;
      } // add zero in front of numbers < 10
      return i;
    },
    // 12-hour time formatter here:
    // https://stackoverflow.com/questions/8888491/how-do-you-display-javascript-datetime-in-12-hour-am-pm-format
    formatAMPM: function() {
      var today = new Date();
      var hours = today.getHours();
      var minutes = today.getMinutes();
      var seconds = today.getSeconds();
      var ampm = hours >= 12 ? "pm" : "am";
      hours = hours % 12;
      hours = hours ? hours : 12; // the hour '0' should be '12'
      minutes = this.checkTime(minutes);
      seconds = this.checkTime(seconds);
      this.time = hours + ":" + minutes + ":" + seconds + " " + ampm;
      setTimeout(this.formatAMPM, 500);
    }
  },
  // before the component mounts
  beforeMount() {
    // if (!this.twelveHour) {
    //   this.formatAMPM();
    // } else {
    //   this.startTime();
    // }
  },
  // when the component mounts
  mounted() {
    document.getElementById("search-box").focus();
    if (localStorage.name) {
      this.name = localStorage.name;
    }
    if (localStorage.lightMode) {
      this.lightMode = localStorage.lightMode;
    }
    if (localStorage.twelveHour) {
      this.twelveHour = localStorage.twelveHour;
    }

    if (this.twelveHour === "true") {
      this.formatAMPM();
    } else {
      this.startTime();
    }
  },
  watch: {
    name(newName) {
      localStorage.name = newName;
    },
    lightMode(newLightMode) {
      localStorage.lightMode = newLightMode;
    },
    twelveHour(newTwelveHour) {
      localStorage.twelveHour = newTwelveHour;
    }
  }
};
</script>

<style scoped>
.main-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  height: 100vh;
}

.search-element {
  display: flex;
}

.columns > * {
  border-radius: 5px;
  margin: 20px 10px;
}

.settings-button {
  position: absolute;
  right: 0;
  margin: 25px;
}

.settings-area {
  position: absolute;
  z-index: 5;
  right: 0;
  top: 110px;
  margin: 0 25px 0 0;
  padding: 15px;
  background-color: #44475a;
  box-shadow: 2px 2px 5px 2px rgba(0, 0, 0, 0.25);
  /* border: 2px solid #44475a; */
  border-radius: 5px;
  height: 400px;
  width: 250px;
}

.settings-label {
  color: white;
}

.settings-save-button {
  bottom: 10px;
  left: 10px;
  right: 10px;
  width: 92%;
  position: absolute;
}

.column-add-button {
  bottom: 10px;
  left: 10px;
  right: 10px;
  width: 97%;
  position: absolute;
}

.add-column-menu {
  position: absolute;
  z-index: 5;
  /* top: 430px;
  left: 245px;
  width: 500px;
  height: 300px; */
  padding: 15px;
  border-radius: 5px;
  background-color: #44475a;
  box-shadow: 2px 2px 10px 5px rgba(0, 0, 0, 0.45);
  margin-left: 50px;
  margin-right: 50px;
  left: 550px;
  right: 550px;
  top: 350px;
  bottom: 350px;
}

.close-column-button {
  position: absolute;
  top: 10px;
  right: 10px;
}

.modal-background {
  z-index: 5;
}
</style>
