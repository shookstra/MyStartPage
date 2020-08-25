<template>
  <div>
    <div class="modal-background" v-if="newBookmarkForm"></div>

    <b-button @click="categorizeBookmarks">Categorize Bookmarks</b-button>

    <!-- Action buttons section -->
    <transition name="fade">
      <!-- container for holding buttons and elements relating to the action buttons -->
      <div class="action-buttons-container">
        <b-button
          @click="actionButtonsToggled = !actionButtonsToggled"
          class="toggle-action-buttons"
          type="is-primary"
          size="is-large"
          icon-left="dots-horizontal"
        />
        <!-- action buttons -->
        <transition name="translate-y">
          <div v-if="actionButtonsToggled" class="action-buttons">
            <b-button
              @click="toggleAddBookmarkModal"
              class="new-column-button"
              type="is-primary"
              size="is-large"
              icon-left="plus"
            />
            <b-button
              @click="settingsToggled = !settingsToggled"
              class="settings-button"
              type="is-primary"
              size="is-large"
              icon-left="cog"
            />
          </div>
        </transition>
      </div>
    </transition>
    <!-- Area where settings options are located -->
    <transition name="fade">
      <div v-if="settingsToggled" class="settings-area">
        <p class="title is-size-4 has-text-light has-text-centered">Settings</p>
        <div class="field">
          <label for>Name</label>
          <b-input v-model="name"></b-input>
        </div>
        <div class="field">
          <b-switch
            v-model="twelveHour"
            passive-type="is-dark"
            type="is-primary"
            >12-hour Clock</b-switch
          >
        </div>
        <div class="field">
          <b-switch
            v-model="lightMode"
            passive-type="is-dark"
            type="is-warning"
            true-value="Light Mode (WIP)"
            false-value="Dark Mode (WIP)"
            >{{ lightMode }}</b-switch
          >
        </div>
        <b-button
          @click="reloadPage"
          class="is-primary settings-save-button"
          size="is-medium"
          icon-left="content-save-settings"
          >Save</b-button
        >
      </div>
    </transition>

    <!-- Area where the add column modal is located -->
    <div v-if="addColumnToggled" class="add-column-menu">
      <b-button
        class="is-danger close-column-button"
        size="is-medium"
        icon-left="close"
        @click="addColumnToggled = !addColumnToggled"
      ></b-button>
      <p class="title is-size-4 has-text-light has-text-centered">
        Add New Column
      </p>
      <div class="field">
        <label for="columnName" class="is-size-5">Column Name</label>
        <b-input v-model="columnName"></b-input>
      </div>
      <b-button
        @click="addColumn"
        class="is-primary column-add-button"
        size="is-medium"
        icon-left="plus-circle"
        >Add Column</b-button
      >
    </div>

    <div class="main-container container">
      <p v-if="name === ''" class="is-size-1 has-text-white">
        Let's Get Started
      </p>
      <p v-if="name" class="is-size-1 has-text-white">Welcome, {{ name }}</p>
      <p class="title has-text-white is-size-4">{{ time }}</p>

      <!-- <ul v-for="bookmark in bookmarks" :key="bookmark.name">
        <Bookmark
          :bookmark="bookmark.bookmark"
          :url="bookmark.url"
          :columnName="bookmark.columnName"
        />
      </ul> -->

      <transition name="translate">
        <div
          v-if="newBookmarkForm"
          class="modal-background new-bookmark-section"
        >
          <h1 class="is-size-4 create-new-bookmark-title">
            Create new Bookmark
          </h1>
          <b-button
            class="is-danger close-column-button"
            size="is-medium"
            icon-left="close"
            @click="newBookmarkForm = !newBookmarkForm"
          ></b-button>
          <!-- Bookmark name section -->
          <div class="field">
            <p class="control has-icons-left has-icons-right">
              <input
                class="input"
                type="text"
                placeholder="Enter your bookmark name"
                v-model="bookmark"
              />
              <span class="icon is-small is-left">
                <i class="fas fa-bookmark"></i>
              </span>
            </p>
          </div>
          <!-- URL Section -->
          <div class="field">
            <p class="control has-icons-left">
              <input
                class="input"
                type="text"
                placeholder="Enter your bookmark URL"
                v-model="url"
              />
              <span class="icon is-small is-left">
                <i class="fas fa-code"></i>
              </span>
            </p>
          </div>
          <!-- Column name section -->
          <div class="field">
            <p class="control has-icons-left has-icons-right">
              <input
                class="input"
                type="text"
                placeholder="Enter your column name"
                v-model="columnName"
              />
              <span class="icon is-small is-left">
                <i class="fas fa-columns"></i>
              </span>
            </p>
          </div>

          <button
            class="button is-primary is-medium is-fullwidth"
            @click="createBookmark"
          >
            Create Bookmark
          </button>
        </div>
      </transition>

      <ul v-if="!Array.isArray(bookmarks)" class="bookmark-list bookmark-item">
        <li>Single Item</li>
        <li>URL: {{ bookmarks.url }}</li>
        <li>Bookmark: {{ bookmarks.bookmark }}</li>
        <li>Column Name: {{ bookmarks.columnName }}</li>
      </ul>
      <ul
        v-else
        v-for="bookmark in bookmarks"
        :key="bookmark.bookmark"
        class="bookmark-list bookmark-item"
      >
        <li>URL: {{ bookmark.url }}</li>
        <li>Bookmark: {{ bookmark.bookmark }}</li>
        <li>Column Name: {{ bookmark.columnName }}</li>
      </ul>

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

      <div v-for="column in bookmarks" :key="column.columnName">
        <Column :name="column.columnName" :bookmarks="column.bookmarks" />
        <div>
          <h1 class="is-size-4">{{ column.columnName }}</h1>
          <div v-for="bookmark in column.bookmarks" :key="bookmark.bookmark">
            <p>{{ bookmark.bookmark }}</p>
          </div>
          <p></p>
        </div>
      </div>

      <!-- Bookmark category columns -->
      <div class="columns">
        <Column name="Social Media" :links="socialMediaLinks" />
        <Column name="Games" :links="gameLinks" />
        <Column name="Programming" :links="programmingLinks" />
        <Column name="Misc." :links="miscLinks" />
      </div>
      <b-button
        class="is-primary add-button"
        size="is-medium"
        icon-left="plus-circle"
        >Add Column</b-button
      >
      <div class="notification is-primary">
        <button class="delete"></button>
        My-Start-Page uses the browser's local storage to store your columns and
        bookmarks. Deleting this or clearing your browser cache could delete
        your data.
      </div>

      <transition name="translate" tag="div">
        <div
          class="bottom-messages notification is-danger"
          id="error-messages"
          v-if="errorMessages.length > 0"
        >
          <button
            class="delete"
            id="error-message-close"
            @click="errorMessages = ''"
          ></button>
          <ul v-for="errorMessage in errorMessages" :key="errorMessage">
            <li>{{ errorMessage }}</li>
          </ul>
        </div>
      </transition>

      <transition name="translate">
        <div
          class="bottom-messages notification is-success"
          id="success-message"
          v-if="successfullyAddedBookmark"
        >
          <button
            class="delete"
            @click="successfullyAddedBookmark = false"
          ></button>
          <p>Successfully added new bookmark!</p>
        </div>
      </transition>
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
      bookmark: "",
      url: "",
      bookmarks: [],
      errorMessages: [],
      successfullyAddedBookmark: false,
      settingsToggled: false,
      addColumnToggled: false,
      newBookmarkForm: false,
      actionButtonsToggled: false,
      socialMediaLinks: [
        {
          id: 1,
          name: "Reddit (Old)",
          link: "https://old.reddit.com",
        },
        {
          id: 2,
          name: "Twitter",
          link: "https://www.twitter.com",
        },
        {
          id: 3,
          name: "Outlook",
          link:
            "https://www.microsoft.com/en-us/microsoft-365/outlook/email-and-calendar-software-microsoft-outlook",
        },
        {
          id: 4,
          name: "Gmail",
          link: "https://mail.google.com/mail/u/0/#inbox",
        },
        {
          id: 5,
          name: "YouTube",
          link: "https://www.youtube.com",
        },
      ],
      programmingLinks: [
        {
          id: 1,
          name: "Stack Overflow",
          link: "https://www.stackoverflow.com",
        },
        {
          id: 2,
          name: "MDN",
          link: "https://developer.mozilla.org/en-US/",
        },
      ],
      gameLinks: [
        {
          id: 1,
          name: "Steam",
          link: "https://store.steampowered.com/",
        },
        {
          id: 2,
          name: "IsThereAnyDeal",
          link: "https://isthereanydeal.com/",
        },
        {
          id: 3,
          name: "Chrono.gg",
          link: "https://www.chrono.gg/",
        },
        {
          id: 4,
          name: "Humble Bundle",
          link: "https://www.humblebundle.com/",
        },
        {
          id: 5,
          name: "ProtonDB",
          link: "https://www.protondb.com/",
        },
      ],
      miscLinks: [
        {
          id: 1,
          name: "Bandwidth",
          link: "http://bw.bloombb.net/",
        },
        {
          id: 2,
          name: "Bank",
          link: "https://www.pinnbank.com/",
        },
        {
          id: 3,
          name: "Discover",
          link: "https://www.discover.com/",
        },
      ],
    };
  },
  components: {
    Column,
  },
  methods: {
    addColumn: function() {
      const jsonified = {
        columnName: this.columnName,
        links: "",
      };
      localStorage.setItem("columnName", JSON.stringify(jsonified));
      alert("Add column");
    },
    searchIconClick: function() {
      if (this.searchTerm !== "") {
        window.location.assign(
          "https://www.duckduckgo.com?q=" + this.searchTerm
        );
      }
    },
    // method for toggling the add bookmark modal
    // and hiding the settings menu
    toggleAddBookmarkModal: function() {
      window.scrollTo(0, 0);
      this.newBookmarkForm = !this.newBookmarkForm;
      this.settingsToggled = false;
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
    },
    createBookmark: function() {
      this.validateBookmarkInputs();
      if (this.errorMessages.length === 0) {
        // create a new bookmark object based on user input
        const newBookmark = {
          columnName: this.columnName,
          bookmark: this.bookmark,
          url: this.url,
        };

        if (
          localStorage.getItem("bookmarks") === "" ||
          localStorage.getItem("bookmarks") === null
        ) {
          console.log("No bookmarks in local storage");
          localStorage.setItem("bookmarks", JSON.stringify(newBookmark));
        } else {
          let currentBookmarks = [];
          let unformattedCurrentBookmarks = JSON.parse(
            localStorage.getItem("bookmarks")
          );

          if (Array.isArray(unformattedCurrentBookmarks)) {
            unformattedCurrentBookmarks.forEach((bookmark) => {
              currentBookmarks.push(bookmark);
            });
          } else {
            currentBookmarks.push(unformattedCurrentBookmarks);
          }

          currentBookmarks.push(newBookmark);
          this.bookmarks = currentBookmarks;
          localStorage.setItem("bookmarks", JSON.stringify(this.bookmarks));
          // clear the new bookmark user input fields
          this.clearBookmarkInputs();
          this.successfullyAddedBookmark = true;
        }
      }
    },
    // function to validate bookmark user inputs
    validateBookmarkInputs: function() {
      let errorMessages = [];
      if (this.columnName === "") {
        errorMessages.push("Please enter a column name");
      }
      if (this.url === "") {
        errorMessages.push("Please enter a URL");
      }
      if (this.bookmark === "") {
        errorMessages.push("Please enter a name for you bookmark");
      }
      this.errorMessages = errorMessages;
    },
    clearBookmarkInputs: function() {
      this.bookmark = "";
      this.url = "";
      this.columnName = "";
    },
    categorizeBookmarks: function() {
      // reorganized array of bookmarks grouped by column name
      let organizedColumns = [];
      // array of column names for checking if the bookmark has been added already
      let columnNames = [];
      // loop through each bookmark in state
      this.bookmarks.forEach((bookmark) => {
        // if there are no bookmarks in the array yet add an initial bookmark
        if (organizedColumns.length === 0) {
          organizedColumns.push({
            columnName: bookmark.columnName,
            bookmarks: [{ bookmark: bookmark.bookmark, url: bookmark.url }],
          });
          // append to the column names array
          columnNames.push(bookmark.columnName);
        } else {
          // loop through each element in the organized columns object array
          organizedColumns.forEach((column, index) => {
            // if the current bookmark's column name is in the array of column names append to the existing array
            if (columnNames.includes(bookmark.columnName)) {
              organizedColumns[index].bookmarks.push({
                bookmark: bookmark.bookmark,
                url: bookmark.url,
              });
            } else {
              // else create a new object in the array of columns
              organizedColumns.push({
                columnName: bookmark.columnName,
                bookmarks: [{ bookmark: bookmark.bookmark, url: bookmark.url }],
              });
              // append to the column names array
              columnNames.push(bookmark.columnName);
            }
          });
        }
      });

      console.dir(organizedColumns);
      localStorage.setItem("bookmarks", JSON.stringify(organizedColumns));
    },
  },
  // when the component mounts
  mounted() {
    // const bookmark = localStorage.getItem("bookmarks");
    if (localStorage.getItem("bookmarks") !== null) {
      this.bookmarks = JSON.parse(localStorage.getItem("bookmarks"));
    }
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
    },
  },
};
</script>

<style scoped>
.main-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  margin-top: auto;
  min-height: 90vh;
}

.search-element {
  display: flex;
}

.columns > * {
  border-radius: 5px;
  margin: 20px 10px;
}

.settings-button {
  z-index: 1;
  /* position: absolute;
  right: 0;
  margin: 90px 25px 0 0; */
}

.new-column-button {
  z-index: 1;
  /* position: absolute;
  right: 0;
  margin: 25px; */
}

.settings-area {
  position: absolute;
  z-index: 5;
  right: 0;
  top: 55px;
  right: 75px;
  margin: 0 25px 0 0;
  padding: 15px;
  background-color: #44475a;
  box-shadow: 2px 2px 5px 2px rgba(0, 0, 0, 0.25);
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
  padding: 15px;
  border-radius: 5px;
  background-color: #44475a;
  box-shadow: 2px 2px 10px 5px rgba(0, 0, 0, 0.45);
  margin-left: 50px;
  margin-right: 50px;
  height: 200px;
  left: 20%;
  right: 20%;
  top: 40%;
}

.close-column-button {
  position: absolute;
  top: 10px;
  right: 10px;
}

.modal-background {
  z-index: 5;
}

.notification {
  margin-top: 15px;
}

.bookmark-list {
  margin: 5px 0 5px 0;
  padding: 10px;
}

.bookmark-item {
  background-color: #6272a4;
  border-radius: 5px;
}

.bottom-messages {
  position: fixed;
  left: 25px;
  right: 25px;
  bottom: 15px;
  z-index: 5;
}

.new-bookmark-section {
  position: absolute;
  z-index: 5;
  left: 50px;
  right: 50px;
  top: 25%;
  height: 275px;
  padding: 10px;
  background-color: #44475a;
  border-radius: 5px;
}

.divider {
  margin: 25px 0 25px 0;
  border-top: 5px solid #44475a;
}

.create-new-bookmark-title {
  margin-bottom: 20px;
}

.action-buttons-container {
  position: absolute;
  right: 100px;
}

.action-buttons {
  z-index: 4;
  position: absolute;
  display: flex;
  flex-direction: column;
  background-color: #44475a;
  height: 150px;
  padding: 10px;
  border-radius: 5px;
  /* right: 25px; */
  right: -80px;
  top: 0;
}

.action-buttons > * {
  margin: 5px 0;
}

.toggle-action-buttons {
  z-index: 2;
}

/* transitions */
.translate-enter-active,
.translate-leave-active {
  transition: transform 350ms;
}

.translate-enter, .translate-leave-to /* .fade-leave-active below version 2.1.8 */ {
  transform: translateY(1000px);
}

.translate-y-enter-active,
.translate-y-leave-active {
  transition: transform 250ms;
}

.translate-y-enter, .translate-y-leave-to /* .fade-leave-active below version 2.1.8 */ {
  transform: translateX(100px);
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 250ms;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}

@media only screen and (max-width: 900px) {
  .add-column-menu {
    border: 1px solid red;
    left: 5%;
    right: 5%;
  }
}

@media only screen and (max-width: 630px) {
  .column-add-button {
    width: 95%;
  }
}

@media only screen and (max-width: 375px) {
  .add-column-menu {
    left: 0%;
    right: 0%;
    margin: 0 10px;
  }
}
</style>
