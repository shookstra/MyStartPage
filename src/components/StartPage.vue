<template>
  <div class="start-page">
    <p class="is-size-1 has-text-white">Start Page</p>
    <p class="title has-text-white is-size-4">{{ time }}</p>
    <b-field>
      <b-input
        placeholder="Duckduckgo Search..."
        type="search"
        icon-right="magnify"
        icon-right-clickable
        size="is-large"
        v-model="searchTerm"
        @icon-right-click="searchIconClick"
      ></b-input>
    </b-field>
    <div class="columns">
      <Column name="Social Media" :links="socialMediaLinks" />
      <Column name="Games" :links="gameLinks" />
      <Column name="Programming" :links="programmingLinks" />
      <Column name="Misc." :links="miscLinks" />
    </div>
    <!-- <b-button
      rounded
      class="is-primary add-button"
      size="is-medium"
      icon-left="plus"
      @click="addColumn"
    >Add Column</b-button>-->
  </div>
</template>

<script>
import Column from "./Column.vue";

export default {
  name: "StartPage",
  data: function() {
    return {
      searchTerm: "",
      time: "",
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
      alert("Add column");
    },
    searchIconClick: function() {
      if (this.searchTerm !== "") {
        window.open("https://www.duckduckgo.com?q=" + this.searchTerm);
      }
    },
    // time functions from
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
    }
  },
  beforeMount() {
    this.startTime();
  }
};
</script>

<style scoped>
.start-page {
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
</style>
