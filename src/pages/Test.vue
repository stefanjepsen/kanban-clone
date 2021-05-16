<template>
  <q-page>
    <!-- page content -->

    <div class="q-pa-md row">
      <div class="col-8 offset-2">
        <div class="row q-gutter-xs">
          <h3>Hej</h3>

          <q-item
            v-for="project in projects"
            :key="project.id"
            class="qweetStyle q-py-md"
          >
            <h3>{{ project.title }}</h3>
          </q-item>
        </div>
      </div>
    </div>
    <div class="q-pa-md row q-gutter-md"></div>
  </q-page>
</template>

<script>
import { db } from "boot/firebase.js";

export default {
  name: "Test",
  data() {
    return {
      projects: [],
      users: [],
    };
  },

  mounted() {
    db.collection("projects").onSnapshot((snapshot) => {
      snapshot.docChanges().forEach((change) => {
        let projectsChange = change.doc.data();

        if (change.type === "added") {
          console.log("New project: ", projectsChange);
          this.projects.unshift(projectsChange);
        }
        if (change.type === "modified") {
          console.log("Modified city: ", projectsChange);
        }
        if (change.type === "removed") {
          console.log("Removed city: ", projectsChange);
        }
      });
    });
    // Add a new document in collection "cities"
    db.collection("cities")
      .doc("LA")
      .set({
        name: "Los Angeles",
        state: "CA",
        country: "USA",
      })
      .then(() => {
        console.log("Document successfully written!");
      })
      .catch((error) => {
        console.error("Error writing document: ", error);
      });

    var cityRef = db.collection("cities").doc("LA");

    var setWithMerge = cityRef.set(
      {
        capital: true,
        diller: false
      },
      { merge: true }
    );
  },

  /*   data: () => ({
    events: [],
  }),
  mounted() {
    this.getEvents();
  },
  methods: {
    async getEvents() {
        let snapshot = await db.collection('projects').get()
        let events = []
        snapshot.forEach(doc => {
            //console.log(doc.data())
            let appData = doc.data()
            appData.id = doc.id
            events.push(appData)
        }),
        this.events = events;
    },
  }, */
};
</script>

<style>
</style>