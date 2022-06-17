<template>
  <div class="pa-3">
    <p class="text-center text-h3 mb-8">Our Kanban board</p>
    <v-row>
      <v-col v-for="(person, personIndex) in people" :key="personIndex">
        <div class="d-flex pa-2 rounded-sm" :class="person.color">
          <h3>{{ person.name }}</h3>
          <v-spacer></v-spacer>
          <v-icon @click="openDialog(personIndex)">mdi-plus</v-icon>
        </div>
        <draggable
          class="list-group"
          :list="person.list"
          group="people"
          @change="log"
        >
          <div
            class="list-group-item d-flex align-center"
            v-for="(element, elIndex) in person.list"
            :key="element.id"
          >
            {{ element.name }}
            <v-spacer></v-spacer>
            <v-btn
              @click="deleteItem(personIndex, elIndex)"
              color="red lighten-5"
              fab
              x-small
              text
            >
              <v-icon>mdi-delete</v-icon>
            </v-btn>
            <v-btn
              @click="editItem(personIndex, elIndex)"
              color="grey lighten-3"
              fab
              x-small
              text
            >
              <v-icon>mdi-pencil</v-icon>
            </v-btn>
          </div>
        </draggable>
      </v-col>
    </v-row>

    <v-dialog
      v-model="dialog"
      scrollable
      :overlay="false"
      max-width="500px"
      transition="dialog-transition"
    >
      <v-card>
        <v-card-title class="primary">
          <span class="white--text">{{ selectedPerson.name }}</span>
        </v-card-title>
        <v-card-text class="pt-6">
          <v-text-field
            v-model="jobText"
            label="Job description"
            outlined
          ></v-text-field>
        </v-card-text>
        <v-card-actions class="px-6">
          <v-btn @click="addJob" color="success">Add job</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- <rawDisplayer class="col-3" :value="list1" title="Ci" />
    <rawDisplayer class="col-3" :value="list2" title="Chris" />
    <rawDisplayer class="col-3" :value="list3" title="Owen" />
    <rawDisplayer class="col-3" :value="list4" title="Claudio" /> -->
  </div>
</template>
<script>
import draggable from "vuedraggable";
export default {
  name: "two-lists",
  display: "Two Lists",
  order: 1,
  components: {
    draggable,
  },
  data() {
    return {
      dialog: false,
      selectedPerson: {
        name: "",
        index: null,
      },
      jobText: "",
      people: [
        {
          name: "Dylan",
          color: "green",
          list: [
            { name: "Release", id: 1 },
            { name: "UI work", id: 2 },
            { name: "Fixing bugs", id: 3 },
          ],
        },
        {
          name: "Ci",
          color: "red",
          list: [
            { name: "Testing UI", id: 4 },
            { name: "Testing negApp emails", id: 5 },
          ],
        },
        {
          name: "Owen",
          color: "blue",
          list: [
            { name: "UI work", id: 6 },
            { name: "Pre release stuff", id: 7 },
          ],
        },
        {
          name: "Claudio",
          color: "orange",
          list: [
            { name: "Component testing", id: 10 },
            { name: "UX meeting", id: 11 },
            { name: "Complete Lambda function", id: 12 },
          ],
        },
        {
          name: "Chris",
          color: "yellow",
          list: [
            { name: "App work", id: 8 },
            { name: "API release", id: 9 },
          ],
        },
      ],
    };
  },
  methods: {
    deleteItem(person, element) {
      this.people[person].list.splice(element, 1);
      console.log(person, element);
    },
    editItem(person, element) {
      this.selectedPerson.name = this.people[person].name;
      this.selectedPerson.index = person;
      this.jobText = this.people[person].list[element].name;
      this.dialog = true;
    },
    addJob() {
      this.people[this.selectedPerson.index].list.push({
        name: this.jobText,
        id: this.randomID,
      });

      this.jobText = "";
      this.selectedPerson.name = "";
      this.selectedPerson.index = null;
      this.dialog = false;
    },
    add: function () {
      this.list.push({ name: "Juan" });
    },
    replace: function () {
      this.list = [{ name: "Edgard" }];
    },
    clone: function (el) {
      return {
        name: el.name + " cloned",
      };
    },
    log: function (evt) {
      window.console.log(evt);
    },
    openDialog(person) {
      this.selectedPerson.name = this.people[person].name;
      this.selectedPerson.index = person;
      this.dialog = true;
    },
  },
  computed: {
    randomID() {
      return "_" + Math.random().toString(36).substr(2, 9);
    },
  },
};
</script>

<style scoped>
.list-group-item {
  border: 1px solid grey;
  border-radius: 3px;
  margin: 10px 0;
  padding: 0.5rem;
}
</style>
