<template>
  <div>
    <h2>Property Information</h2>
    <form class="form-inline text-center">
      <label>Address:</label>
      <input class="form-control" v-model="address" title="address" required>
      <label>Zipcode:</label>
      <input class="form-control" v-model="zipcode" title="zipcode" required>
      <button class="btn-primary" v-on:click.stop="getData()">submit</button>
    </form>
    <span class="alert-warning center-block" v-if="notExist">The input address and zipcode pair not exist !</span>
    <table>
      <tr v-for="(value, key, index) in res" v-if="value" :key="index">
        <td>{{trans(key)}}</td>
        <td>{{ value }}</td>
      </tr>
    </table>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'index',
  data () {
    return {
      posts: {},
      errors: [],
      res: {},
      buf: {},
      address: '43 Valmonte Plaza',
      zipcode: '90274',
      notExist: false
    }
  },
  async created () {
    await this.getData()
  },
  methods: {
    trans (key) {
      return key.replace(/_/g, ' ')
    },
    async getData () {
      try {
        const response = await
          axios.get('https://api.housecanary.com/v2/property/details', {
            auth: {
              username: 'CE9T3CWBN9FMO6V4DUXQ',
              password: 'IspDvliUrQnlI9KtWacb2NeZxkeOTMmU'
            },
            params: {
              address: this.address,
              zipcode: this.zipcode
            }
          })
        this.posts = response.data
      } catch (error) {
        console.error(error)
      }
      this.buf = this.posts[0]['property/details']
      if (this.buf['api_code'] === 0) {
        this.notExist = false
        this.res = this.buf['result']['property']
      } else {
        this.notExist = true
      }
    }
  }
}
</script>

<style scoped>
  table {
    width: 80%;
    text-align: center;
    margin: auto;
  }

  td {
    padding-top: 20px;
    padding-bottom: 20px;
  }

  tr:nth-child(even) {
    background-color: #b9def0;
  }

  h2 {
    text-align: center;
  }

  form {
    margin: 30px auto;
  }

  .center-block {
    width: 40%;
    text-align: center;
  }
</style>
