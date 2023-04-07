<template>
  <v-popup v-if="isInfoPopupVisible" @closePopup="closeInfoPopup">
    <div>
      <p class="info__text">{{ name }}</p>
      <p class="info__text">{{ lastName }}</p>
      <p class="info__text">{{ email }}</p>
      <p class="info__text">{{ phone }}</p>
      <button @click="handleDelete(t)" class="button btn-centr">Delete</button>
    </div>
  </v-popup>
  <nav class="header">
    <div class="container header__wrap">
      <h1 class="header__logo">Dashboard</h1>
      <div class="header__registration">
        <a class="header__link" href="">Sign in</a>
        <a class="header__link" href="">Sign up</a>
      </div>
    </div>
  </nav>
  <div class="container">
    <div class="wrapper">
      <div class="wrap">
        <div class="wrap__form">
          <p class="wrap__form-title">Сreate</p>
          <div class="wrap__input">
            <input
              v-model="ticker"
              v-on:keydown.enter="add"
              type="text"
              name="wallet"
              id="wallet"
              placeholder="First name"
              class="wrap__form-inp"
            />
          </div>
          <div class="wrap__input">
            <input
              v-model="lastName"
              v-on:keydown.enter="add"
              type="text"
              placeholder="Last Name"
              class="wrap__form-inp"
            />
          </div>
          <div class="wrap__input">
            <input
              v-model="email"
              type="email"
              name="email"
              placeholder="Email"
              class="wrap__form-inp"
            />
          </div>
          <div class="wrap__input">
            <input
              v-model="phone"
              type="text"
              id="phone"
              placeholder="+380505454629"
              class="wrap__form-inp"
            />
          </div>
          <button @click="add" class="button black">Add</button>
        </div>
        <div class="wrap__input">
          <input v-model="filter" placeholder="Filter" class="wrap__form-inp" />
        </div>
      </div>
      <template v-if="tickers.length > 0">
        <div class="info">
          <ul class="info__card">
            <li v-for="t in filteredTickers()" :key="t.name" class="info__item">
              <p class="info__text">{{ t.name }}</p>
              <p class="info__text">{{ t.lastName }}</p>
              <p class="info__text">{{ t.email }}</p>
              <p class="info__text">{{ t.phone }}</p>
              <button @click="handleDelete(t)" class="button red btn-centr">
                Delete
              </button>
              <button
                @click="showPopupInfo"
                class="button black btn-centr-centr"
              >
                Show info
              </button>
            </li>
          </ul>
          <div class="info__pag">
            <button
              v-if="page > 1"
              @click="page = page - 1"
              class="button black"
            >
              Previous
            </button>
            <button
              v-if="hasNextPage"
              @click="page = page + 1"
              class="button black"
            >
              Next
            </button>
          </div>
        </div>
      </template>
    </div>
  </div>
</template>

<script>
import vPopup from "./components/popup.vue";
export default {
  name: "App",
  props: {
    cart_item_data: {
      type: Object,
      default() {
        return {};
      },
    },
  },
  components: {
    vPopup,
  },
  data() {
    return {
      isInfoPopupVisible: false,
      ticker: "",
      lastName: "",
      email: "",
      phone: "",
      tickers: [
        {
          name: "Vladislav",
          lastName: "Ryabov",
          email: "ryabov.it@gmail.com",
          phone: "+380508064679",
        },
        {
          name: "Andry",
          lastName: "Myers",
          email: "andrymyers@gmail.com",
          phone: "+380668444679",
        },
        {
          name: "Lauren",
          lastName: "Clark",
          email: "lauren.it@gmail.com",
          phone: "+380502844679",
        },
        {
          name: "Carolyn",
          lastName: "Green",
          email: "green@gmail.com",
          phone: "+380508444789",
        },
        {
          name: "Arthur",
          lastName: "White",
          email: "arthur@gmail.com",
          phone: "+380508445679",
        },
        {
          name: "Anna",
          lastName: "Thompson",
          email: "thompson.it@gmail.com",
          phone: "+380508454679",
        },
        {
          name: "Brittany",
          lastName: "Newman",
          email: "brittany@gmail.com",
          phone: "+380523444679",
        },
        {
          name: "Roy",
          lastName: "Long",
          email: "roy.long@gmail.com",
          phone: "+380512444679",
        },
      ],
      page: 1,
      filter: "",
      hasNextPage: true,
    };
  },
  created() {
    const windowData = Object.fromEntries(
      new URL(window.location).searchParams.entries()
    );

    if (windowData.filter) {
      this.filter = windowData.filter;
    }

    if (windowData.page) {
      this.page = windowData.page;
    }
  },
  methods: {
    showPopupInfo() {
      this.isInfoPopupVisible = true;
    },
    closeInfoPopup() {
      this.isInfoPopupVisible = false;
    },
    filteredTickers() {
      const start = (this.page - 1) * 6;
      const end = this.page * 6;

      const filteredTickers = this.tickers.filter((ticker) =>
        ticker.name.includes(this.filter)
      );
      this.hasNextPage = filteredTickers.length > end;
      return filteredTickers.slice(start, end);
    },
    add() {
      const newTicker = {
        name: this.ticker,
        lastName: this.lastName,
        email: this.email,
        phone: this.phone,
      };
      this.tickers.push(newTicker);
      this.ticker = "";
      this.lastName = "";
      this.email = "";
      this.phone = "";
      this.filter = "";
    },
    handleDelete(tickerToRemove) {
      this.tickers = this.tickers.filter((t) => t != tickerToRemove);
    },
  },
  watch: {
    filter() {
      this.page = 1;
      window.history.pushState(
        null,
        document.title,
        `${window.location.pathname}?filter=${this.filter}&page=${this.page}`
      );
    },
    page() {
      window.history.pushState(
        null,
        document.title,
        `${window.location.pathname}?filter=${this.filter}&page=${this.page}`
      );
    },
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Acme&family=Roboto:wght@300;400;700&display=swap");
body {
  background: rgba(239, 239, 239, 0.729);
  margin: 0;
  padding: 0;
}
div,
p,
form,
input,
a,
span,
button {
  box-sizing: border-box;
}
ul,
li {
  display: block;
  padding: 0;
  margin: 0;
}
a,
a:hover,
a:active {
  text-decoration: none;
}
input,
input:hover,
input:focus,
input:active,
button,
button:hover,
button:focus,
button:active {
  outline: none;
}

.container {
  max-width: 1400px;
  margin: auto;
}

@media screen and (max-width: 1420px) {
  .wrapper {
    padding: 0 10px;
  }
}

.header {
  background-color: #353535;
  &__wrap {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 0;

    @media screen and (max-width: 1420px) {
      padding: 10px 10px !important;
    }
  }
  &__logo {
    font-family: "Acme";
    font-weight: 400;
    font-size: 30px;
    color: white;
    margin: 0;
  }
  &__link {
    font-family: "Roboto";
    font-weight: 300;
    margin-left: 20px;
    color: white;
    font-size: 15px;

    @media screen and (max-width: 750px) {
      font-size: 12px;
    }
  }
}

.wrap {
  display: flex;
  justify-content: space-around;
  padding: 20px 10px;
  background: rgb(224 224 224);
  margin-bottom: 20px;
  margin-top: 20px;
  border-radius: 5px;
  align-items: center;

  @media screen and (max-width: 900px) {
    flex-direction: column;
    gap: 20px;
  }

  &__form-title {
    font-family: "Roboto";
    font-weight: 400;
    font-size: 15px;
    margin: 0;
  }
  &__input {
    display: flex;
    gap: 20px;
    align-items: center;
  }
  &__input-title {
    font-family: "Roboto";
    font-weight: 400;
    font-size: 15px;
  }
  &__form-inp {
    border: none;
    padding: 5px;
    border-radius: 4px;

    @media screen and (max-width: 800px) {
      width: 270px;
    }
  }
  &__form {
    display: flex;
    gap: 30px;
    align-items: center;

    @media screen and (max-width: 800px) {
      flex-direction: column;
    }
  }
}

.info {
  &__item {
    display: grid;
    grid-template-columns: repeat(6, 1fr);
    padding: 10px 0;
    background: rgb(250, 250, 250);
    margin-bottom: 5px;
    border-radius: 5px;
    align-items: center;

    @media screen and (max-width: 800px) {
      grid: none;
      gap: 10px;
      align-items: center;
    }
  }
  &__item:hover {
    box-shadow: 0px 1px 3px 0px rgba(0, 0, 0, 0.5);
    background: white;
  }
  &__text {
    font-family: "Roboto";
    font-weight: 300;
    // margin-left: 20px;
    font-size: 15px;
    justify-self: center;

    // @media screen and (max-width: 750px) {
    //   margin-left: 0px !important;
    // }
  }
  &__pag {
    display: flex;
    justify-content: center;
    margin-top: 20px;
  }
}

.btn-centr {
  justify-self: end;
  margin-right: 15px;

  @media screen and (max-width: 800px) {
    justify-self: center;
    margin-right: 0px;
  }
}

.btn-centr-centr {
  @media screen and (max-width: 800px) {
    justify-self: center;
  }
}

.button {
  border: none;
  border-radius: 5px;
  font-family: "Roboto";
  font-weight: 300;
  font-size: 11px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  width: 100px;
  height: 22px;
}
.black {
  background-color: #353535;
  border: 1px solid #353535;
  color: white;
}
.black:hover {
  background-color: white;
  color: #353535;
}
.red {
  background-color: #d300008c;
  border: 1px solid #d300008c;
  color: white;
}
.red:hover {
  background-color: white;
  color: #353535;
}
</style>
