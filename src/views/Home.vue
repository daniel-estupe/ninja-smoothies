<template>
  <div class="home container">
    <div class="card" v-for="s in smoothies" :key="s.id">
      <div class="card-content">
        <i class="material-icons delete" @click="deleteSmothie(s.id)">delete</i>
        <h2 class="indigo-tex">{{ s.title }}</h2>
        <ul class="ingredients">
          <li v-for="(ingredient, index) in s.ingredients" :key="`ingredient_${index}`">
            <span class="chip">{{ ingredient }}</span>
          </li>
        </ul>
      </div>
      <span class="btn-floating btn-large halfway-fab pink">
        <router-link :to="{name: 'EditSmoothie', params: {smoothie_slug: s.slug}}">
          <i class="material-icons edit">edit</i>
        </router-link>
      </span>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import db from "@/firebase/init";

export default {
  name: "Home",
  components: {},
  data() {
    return {
      smoothies: [
        // {
        //   title: "Ninja Brew",
        //   slug: "ninja-brew",
        //   ingredients: ["bananas", "coffee", "milk"],
        //   id: 1
        // },
        // {
        //   title: "Morning Mood",
        //   slug: "morning-mood",
        //   ingredients: ["mango", "lime", "juice"],
        //   id: 2
        // }
      ]
    };
  },
  methods: {
    deleteSmothie(id) {
      const _this = this;

      db.collection("smoothies")
        .doc(id)
        .delete()
        .then(() => _this.getSmoothies());
    },
    getSmoothies() {
      db.collection("smoothies")
        .get()
        .then(snapshot => {
          let smoothies = [];

          snapshot.forEach(doc => {
            smoothies.push({ id: doc.id, ...doc.data() });
          });

          this.smoothies = smoothies;
        });
    }
  },
  created() {
    this.getSmoothies();
  }
};
</script>

<style>
.home {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-gap: 30px;
  margin-top: 60px;
}
.home h2 {
  font-size: 1.8em;
  text-align: center;
  margin-top: 0;
}
.home .ingredients {
  margin: 30px auto;
}
.home .ingredients li {
  display: inline-block;
}
.home .delete {
  position: absolute;
  top: 4px;
  right: 4px;
  cursor: pointer;
  color: #aaa;
  font-size: 1.4em;
}
</style>