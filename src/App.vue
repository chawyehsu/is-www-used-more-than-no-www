<template>
  <div id="app">
    <github-corner/>
    <p>Is www used more than no-www?</p>
    <template v-if="sites">
      <h1 v-if="!tie">{{ wwwIsUsedMoreThanNowww ? 'YES' : 'NO' }}</h1>
      <h1 :class="{ pad : tie }" v-else>TIE!</h1>
      <p>
        <small v-if="!wwwIsUsedMoreThanNowww && !tie" class="away">
          Only {{ nowwwCount - wwwCount | formatNumber }} {{ nowwwCount - wwwCount === 1 ? 'site' : 'sites'}} away!
        </small>
        <small v-else-if="wwwIsUsedMoreThanNowww && !tie" class="ahead">
          Ahead by {{ wwwCount - nowwwCount | formatNumber }} {{ wwwCount - nowwwCount === 1 ? 'site' : 'sites'}}!
        </small>
      </p>
      <ul>
        <li>
          <a href="https://github.com/h404bi/is-www-used-more-than-no-www/blob/master/public/sites.json" target="_blank" title="View sites use www">
            <www-icon/>
            <span>{{ wwwCount | formatNumber }}</span>
          </a>
        </li>
        <li>
          <a href="https://github.com/h404bi/is-www-used-more-than-no-www/blob/master/public/sites.json" target="_blank" title="View sites don't use www">
            <nowww-icon/>
            <span>{{ nowwwCount | formatNumber }}</span>
          </a>
        </li>
      </ul>
      <span class="reload" @click="reload">
        <svg :class="{ reloading }" xmlns="http://www.w3.org/2000/svg" width="24" height="24"><path fill="#333333" d="M19 8l-4 4h3c0 3.31-2.69 6-6 6a5.87 5.87 0 0 1-2.8-.7l-1.46 1.46A7.93 7.93 0 0 0 12 20c4.42 0 8-3.58 8-8h3l-4-4zM6 12c0-3.31 2.69-6 6-6 1.01 0 1.97.25 2.8.7l1.46-1.46A7.93 7.93 0 0 0 12 4c-4.42 0-8 3.58-8 8H1l4 4 4-4H6z"/><path d="M0 0h24v24H0z" fill="none"/></svg>
      </span>
    </template>
    <template v-else-if="error">
      <h1 class="error">Error</h1>
      <p>
        Couldn't retrieve any data.
        The API rate limits might have kicked in. Just wait a bit and try again.
      </p>
    </template>
    <p v-else>Loading...</p>
  </div>
</template>

<script>
import axios from 'axios'
import GithubCorner from './components/GithubCorner'
import { WwwIcon, NowwwIcon } from './components/icons'

const PROD_ENDPOINT = 'https://cdn.rawgit.com/h404bi/is-www-used-more-than-no-www/91056e55/public/sites.json'
const DEV_ENDPOINT = 'http://127.0.0.1:8080/sites.json'
const DATA_ENDPOINT = process.env.NODE_ENV === 'production' ? PROD_ENDPOINT : DEV_ENDPOINT

export default {
  name: 'App',

  data() {
    return {
      sites: null,
      error: false,
      reloading: false
    }
  },

  components: {
    WwwIcon,
    NowwwIcon,
    GithubCorner
  },

  mounted() {
    this.fetchData()
    if ('ontouchstart' in window || navigator.msMaxTouchPoints) {
      document.body.classList.remove('no-touch')
    }
  },

  computed: {
    wwwIsUsedMoreThanNowww() {
      return this.wwwCount > this.nowwwCount
    },

    wwwCount() {
      return this.sites.www.length
    },

    nowwwCount() {
      return this.sites.nowww.length
    },

    tie() {
      return this.wwwCount === this.nowwwCount
    }
  },

  filters: {
    formatNumber(number) {
      return new Intl.NumberFormat().format(number)
    }
  },

  methods: {
    async fetchData() {
      await axios.get(`${DATA_ENDPOINT}?_t=${Math.round(new Date().getTime())}`).then(res => {
        this.sites = res.data.sites
      }).catch(err => {
        this.error = true
        // eslint-disable-next-line
        console.log(err)
      })
    },

    async reload() {
      if (this.reloading) return
      this.reloading = true
      await this.fetchData()
      setTimeout(() => {
        this.reloading = false
      }, 900)
    }
  }
}
</script>

<style>

* {
  box-sizing: border-box;
}

::selection {
  background: rgba(0,0,0,0);
}
::-moz-selection {
  background: rgba(0,0,0,0);
}

html, body {
  height: 100%;
  margin: 0;
  padding: 0;
}

body {
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
  text-align: center;
  color: #333;
  background: #efefef;
}

#app {
  width: 300px;
  border: 1px solid #dddddd;
  border-radius: 4px;
  background: #ffffff;
  box-shadow: 0 15px 35px rgba(50,50,93,.1), 0 5px 15px rgba(0,0,0,.07);
  overflow: hidden;
  position: relative;
}

h1 {
  font-size: 100px;
  margin: 10px 0;
}

ul {
  margin: 0;
  padding: 0;
  display: flex;
}

li {
  list-style-type: none;
  width: 50%;
}

li a {
  display: flex;
  align-items: center;
  justify-content: center;
  border-top: 1px solid #dddddd;
  padding: 10px;
  text-decoration: none;
  color: #333;
}

.no-touch li:hover {
  background: #eeeeee;
}

li a > svg {
  display: block;
  width: 22px;
  height: 22px;
}

li a > * {
  margin: 5px;
}

li:last-of-type {
  border-left: 1px solid #dddddd;
}

h1.error {
  font-size: 2em;
}

h1.pad {
  margin-bottom: 30px;
}

p {
  padding: 0 1em;
}

.away, .ahead {
  display: block;
  margin-bottom: 40px;
}

.reload {
  position: absolute;
  left: 50%;
  margin-left: -20px;
  bottom: 30px;
  background: #ffffff;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid #dddddd;
  border-radius: 50%;
}

.reload svg {
  transform-origin: center center;
}

.no-touch .reload:hover {
  cursor: pointer;
  background: #eeeeee;
}

.reloading {
  animation: rotate 1s infinite ease-in-out;
}

@keyframes rotate {
  from {
    transform: rotate(0deg);
  }
  to { 
    transform: rotate(-360deg);
  }
}

</style>
