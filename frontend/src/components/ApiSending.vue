<template>
  <div>
    <button @click="send">GraphQLへリクエストする</button>
    <p>{{ responseValue }}</p>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

const API_HOST = "http://localhost:4000";
const data = { responseValue: "" };

export default defineComponent({
  name: "ApiSending",
  data() {
    return data;
  },
  methods: {
    send() {
      var dice = 3;
      var sides = 6;
      var query = `{
  getDie(numSides: 6) {
    rollOnce
    roll(numRolls: 3)
  }
}`;
      fetch(API_HOST + "/graphql", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          Accept: "application/json",
        },
        body: JSON.stringify({
          query,
          variables: { dice, sides },
        }),
      })
        .then((r) => r.json())
        .then((data) => {
          console.log("data returned:", data);
          this.responseValue = JSON.stringify(data);
        });
    },
  },
});
</script>
