<template>
  <div>
    <h1>Firebase FireStore Example</h1>
    <ul>
      <li v-for="city in cities" :key="city.id" style="margin-bottom: 10px;">
        {{city.name}} : {{ city.country }} <br/>
        population: {{ city.population }}
      </li>
    </ul>

    <button @click="createSampleData">add</button>
  </div>
</template>

<script>
import {
  getFirestore,
  collection,
  doc,
  setDoc,
  query,
  where,
  getDocs,
} from "firebase/firestore";

export default {
  data: () => ({
    cities: [],
  }),

  async mounted() {
    this.getSampleData()
  },
  methods: {
    async createSampleData(){
      const db = getFirestore();
      const citiesRef = collection(db, "cities");
      await setDoc(doc(citiesRef, "SF"), {
        name: "San Francisco",
        state: "CA",
        country: "USA",
        capital: false,
        population: 860000,
        regions: ["west_coast", "norcal"],
      });
      await setDoc(doc(citiesRef, "LA"), {
        name: "Los Angeles",
        state: "CA",
        country: "USA",
        capital: false,
        population: 3900000,
        regions: ["west_coast", "socal"],
      });
      await setDoc(doc(citiesRef, "DC"), {
        name: "Washington, D.C.",
        state: null,
        country: "USA",
        capital: true,
        population: 680000,
        regions: ["east_coast"],
      });
      await setDoc(doc(citiesRef, "TOK"), {
        name: "Tokyo",
        state: null,
        country: "Japan",
        capital: true,
        population: 9000000,
        regions: ["kanto", "honshu"],
      });
      await setDoc(doc(citiesRef, "BJ"), {
        name: "Beijing",
        state: null,
        country: "China",
        capital: true,
        population: 21500000,
        regions: ["jingjinji", "hebei"],
      });
      this.getSampleData()
    },

    async getSampleData() {
      const db = getFirestore();
      const q = query(collection(db, "cities"));
      // const q = query(collection(db, "cities"), where("capital", "==", true));

      const querySnapshot = await getDocs(q);
      querySnapshot.forEach((doc) => {
        console.log(doc.data())
        this.cities.push(doc.data());
      });
      
    },
  },
};
</script>
