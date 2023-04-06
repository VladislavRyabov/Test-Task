<template>
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
          <button @click="add" class="button btn">Add</button>
        </div>
        <div class="wrap__input">
          <input v-model="filter" placeholder="Filter" class="wrap__form-inp" />
        </div>
      </div>
      <template v-if="tickers.length > 0">
        <div class="info__card">
          <div v-for="t in filteredTickers()" :key="t.name" class="info__item">
            <div class="info__text">{{ t.name }}</div>
            <div class="info__text">{{ t.lastName }}</div>
            <div class="info__text">{{ t.email }}</div>
            <div class="info__text">{{ t.phone }}</div>
            <button @click="handleDelete(t)" class="button info__btn">
              Delete
            </button>
          </div>
          <div class="test__pag">
            <button v-if="page > 1" @click="page = page - 1" class="button btn">
              Previous
            </button>
            <button
              v-if="hasNextPage"
              @click="page = page + 1"
              class="button btn"
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
export default {
  name: "App",
  data() {
    return {
      ticker: "",
      lastName: "",
      email: "",
      phone: "",
      tickers: [
        {
          name: "Vladislav",
          lastName: "Ryabov",
          email: "ryabov.it@gmail.com",
          phone: "+380508444679",
        },
        {
          name: "Vladislav",
          lastName: "Ryabov",
          email: "ryabov.it@gmail.com",
          phone: "+380508444679",
        },
        {
          name: "Vladislav",
          lastName: "Ryabov",
          email: "ryabov.it@gmail.com",
          phone: "+380508444679",
        },
        {
          name: "Vladislav",
          lastName: "Ryabov",
          email: "ryabov.it@gmail.com",
          phone: "+380508444679",
        },
        {
          name: "Vladislav",
          lastName: "Ryabov",
          email: "ryabov.it@gmail.com",
          phone: "+380508444679",
        },
        {
          name: "Vladislav",
          lastName: "Ryabov",
          email: "ryabov.it@gmail.com",
          phone: "+380508444679",
        },
        {
          name: "Vladislav",
          lastName: "Ryabov",
          email: "ryabov.it@gmail.com",
          phone: "+380508444679",
        },
        {
          name: "Vladislav",
          lastName: "Ryabov",
          email: "ryabov.it@gmail.com",
          phone: "+380508444679",
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

<style>
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
.header {
  background-color: #353535;
}
@media screen and (max-width: 1420px) {
  .wrapper {
    padding: 0 10px;
  }
  .header__wrap {
    padding: 10px 10px !important;
  }
}
.header__wrap {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 0;
}
.header__logo {
  font-family: "Acme";
  font-weight: 400;
  font-size: 30px;
  color: white;
  margin: 0;
}
.header__link {
  font-family: "Roboto";
  font-weight: 300;
  margin-left: 20px;
  color: white;
  font-size: 20px;
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
}
@media screen and (max-width: 900px) {
  .wrap {
    flex-direction: column;
    gap: 20px;
  }
}
.wrap__form {
  display: flex;
  gap: 30px;
  align-items: center;
}
@media screen and (max-width: 750px) {
  .wrap__form {
    flex-direction: column;
  }
  .header__link {
    font-size: 15px;
  }
  .wrap__form-inp {
    width: 270px;
  }
}
.wrap__form-title {
  font-family: "Roboto";
  font-weight: 400;
  font-size: 15px;
  margin: 0;
}
.wrap__input {
  display: flex;
  gap: 20px;
  align-items: center;
}
.wrap__input-title {
  font-family: "Roboto";
  font-weight: 400;
  font-size: 15px;
}
.wrap__form-inp {
  border: none;
  padding: 5px;
  border-radius: 4px;
}
.info__item {
  display: flex;
  justify-content: space-around;
  padding: 20px 0;
  background: rgb(250, 250, 250);
  margin-bottom: 5px;
  border-radius: 5px;
}
@media screen and (max-width: 750px) {
  .info__item {
    flex-direction: column;
    gap: 10px;
    align-items: center;
  }
  .info__text {
    margin-left: 0px !important;
  }
}
.info__item:hover {
  box-shadow: 0px 1px 3px 0px rgba(0, 0, 0, 0.5);
  background: white;
}
.info__text {
  font-family: "Roboto";
  font-weight: 300;
  margin-left: 20px;
  font-size: 15px;
}
.test__pag {
  display: flex;
  justify-content: center;
  margin-top: 20px;
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
  padding: 4px 20px;
  background-color: #353535;
  border: 1px solid #353535;
  color: white;
}
.button:hover {
  background-color: white;
  color: #353535;
}
</style>
