<template>
  <!-- Table -->
  <v-simple-table fixed-header height="300px" class="mt-8">
    <template v-slot:default>
      <!-- Header row -->
      <thead>
        <tr>
          <th class="text-left">Taiohi</th>
          <th class="text-center">
            Project
          </th>
          <th class="text-center">
            Task 1
          </th>
          <th class="text-center">Task 2</th>
          <th class="text-center">Task 3</th>
          <th class="text-center">Task 4</th>
          <th class="text-center">Task 5</th>
          <th class="text-center">Task 6</th>
          <th class="text-center">Task 7</th>
          <th class="text-center">Task 8</th>
          <th class="text-center">Task 9</th>
          <th class="text-center">Task 10</th>
        </tr>
      </thead>
      <!-- Table body -->
      <tbody>
        <!-- Dynamic student row -->
        <tr
          v-for="(student, studentIndex) in students"
          :key="student.id"
          style="height: 80px;"
        >
          <!-- Profile Pic -->
          <td>
            <v-img :src="student.profilePic" class="profilePic"></v-img>
          </td>

          <!-- Passion Project -->
          <!-- <td>{{ student.given_name }}</td> -->
          <td style="text-align: center;">
            <p v-if="student.passionProject" class="ma-0">
              {{ student.passionProject }}
            </p>
            <!-- NEW -->
            <!-- Dialog Add Passion Project -->
            <v-dialog
              v-else
              v-model="dialogPassion"
              width="500"
              :retain-focus="false"
            >
              <template v-slot:activator="{ on, attrs }">
                <v-btn
                  class="pa-0 ma-0"
                  outlined
                  small
                  color="primary"
                  v-bind="attrs"
                  v-on="on"
                  @click="viewAddPassion(studentIndex)"
                  >+
                </v-btn>
              </template>

              <!-- Dialog -->
              <v-card>
                <v-card-title>
                  Passion Project
                </v-card-title>

                <v-card-text class="mt-4">
                  <p v-if="student.passionProject && !editModePassion">
                    {{ student.passionProject }}
                  </p>
                  <v-text-field
                    v-else
                    v-model="passionProject"
                    label="Name of passion project"
                  ></v-text-field>
                </v-card-text>

                <!-- <v-divider></v-divider> -->

                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="red" text @click="dialogViewTask = false">
                    Cancel
                  </v-btn>

                  <v-btn
                    v-if="editModePassion || !student.passionProject"
                    color="success"
                    text
                    @click="saveEditPassion(studentIndex)"
                  >
                    Save
                  </v-btn>
                  <v-btn
                    v-else
                    color="primary"
                    text
                    @click="editModePassion = true"
                  >
                    Edit
                  </v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
            <!-- End of Dialog -->
          </td>

          <!-- Tasks -->
          <td v-for="(task, taskIndex) in student.tasks" :key="task.id">
            <div class="box">
              <div class="boxSides">
                <!-- Dialog View Task -->
                <v-dialog
                  v-model="dialogViewTask"
                  width="500"
                  :retain-focus="false"
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-btn
                      icon
                      x-small
                      color="primary"
                      v-bind="attrs"
                      v-on="on"
                      @click="viewTask(studentIndex, taskIndex)"
                    >
                      <v-icon>mdi-file-document-outline</v-icon>
                    </v-btn>
                  </template>

                  <!-- Dialog -->
                  <v-card>
                    <v-card-title>
                      Task Link
                    </v-card-title>

                    <v-card-text class="mt-4">
                      <a v-if="!editMode" :href="taskLink">{{ taskLink }}</a>
                      <v-text-field
                        v-else
                        v-model="taskLink"
                        label="Link to Task"
                      ></v-text-field>
                    </v-card-text>

                    <!-- <v-divider></v-divider> -->

                    <v-card-actions>
                      <v-spacer></v-spacer>
                      <v-btn color="red" text @click="dialogViewTask = false">
                        Cancel
                      </v-btn>

                      <v-btn
                        v-if="editMode"
                        color="success"
                        text
                        @click="editTaskLink(studentIndex, taskIndex)"
                      >
                        Save
                      </v-btn>
                      <v-btn
                        v-else
                        color="primary"
                        text
                        @click="editMode = true"
                      >
                        Edit
                      </v-btn>
                    </v-card-actions>
                  </v-card>
                </v-dialog>
                <!-- End of Dialog -->
              </div>
              <div class="boxSides borderLeft">
                <v-btn
                  v-if="task.taskEvidence"
                  icon
                  x-small
                  color="pink"
                  :href="task.taskEvidence"
                >
                  <v-icon>mdi-file-document-edit-outline</v-icon>
                </v-btn>
                <v-btn
                  v-else
                  icon
                  x-small
                  color="grey"
                  :href="task.taskEvidence"
                >
                  <v-icon>mdi-file-document-edit-outline</v-icon>
                </v-btn>
              </div>
              <div class="boxSides borderLeft" :style="{backgroundColor: task.taskCompleted ? 'rgb(76, 175, 80)' : '' }">
                <v-btn
                  v-if="task.taskCompleted"
                  icon
                  x-small
                  color="white"
                  :href="task.taskLink"
                >
                  <v-icon>mdi-check-circle-outline</v-icon>
                </v-btn>
                <v-btn v-else icon x-small color="grey" :href="task.taskLink">
                  <v-icon>mdi-check-circle-outline</v-icon>
                </v-btn>
              </div>
            </div>
          </td>

          <!-- Add Task -->
          <td class="" style="text-align: center;">
            <!-- Dialog Add -->
            <v-dialog v-model="dialogAdd" width="500" :retain-focus="false">
              <template v-slot:activator="{ on, attrs }">
                <v-btn
                  class="pa-0 ma-0"
                  outlined
                  small
                  color="primary"
                  v-bind="attrs"
                  v-on="on"
                  >+
                </v-btn>
              </template>

              <v-card>
                <v-card-title>
                  New Task
                </v-card-title>

                <v-card-text class="mt-4">
                  <v-text-field
                    :v-model="taskLink"
                    label="Link to Task"
                  ></v-text-field>
                </v-card-text>

                <!-- <v-divider></v-divider> -->

                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="red" text @click="dialog = false">
                    Cancel
                  </v-btn>
                  <v-btn color="primary" text @click="save(studentIndex)">
                    Save
                  </v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
          </td>
        </tr>
      </tbody>
    </template>
  </v-simple-table>
</template>

<script>
import Vue from "vue";
import { firestorePlugin } from "vuefire";
import { studentsDb } from "./firebaseInit";

Vue.use(firestorePlugin);

export default {
  name: "StudentTable",
  props: ["yearLevel"],
  data: () => ({
    dialogAdd: false,
    dialogViewTask: false,
    dialogPassion: false,
    taskLink: "",
    passionProject: "",
    viewedTaskLink: "",
    viewingStudent: "",
    viewingTask: "",
    editMode: false,
    editModePassion: false,
    // students: [],
    students: [
      {
        nsn: "1234",
        profilePic: "https://i.pravatar.cc/300",
        passionProject: "",
        tasks: [
          {
            taskLink: "google.com",
            taskEvidence: "twitter.com",
            taskCompleted: true,
          },
          {
            taskLink: "google.com",
            taskEvidence: "",
            taskCompleted: false,
          },
        ],
      },
      {
        nsn: "5678",
        profilePic: "https://i.pravatar.cc/400",
        passionProject: "Carving Project",
        tasks: [
          {
            taskLink: "google.com",
            taskEvidence: "twitter.com",
            taskCompleted: true,
          },
          {
            taskLink: "google.com",
            taskEvidence: "",
            taskCompleted: false,
          },
          {
            taskLink: "google.com",
            taskEvidence: "",
            taskCompleted: false,
          },
        ],
      },
    ],
  }),
  //  firestore: {
  //     students: studentsDb
  //   },
  methods: {
    save(studentIndex) {
      // push TODO: change to firestore db save
      this.students[studentIndex].tasks.push(this.taskLink);
      this.dialogAdd = false;
      alert(
        "saved task link: " +
          this.taskLink +
          " for " +
          this.students[studentIndex].nsn
      );
    },
    saveTaskLink(studentIndex) {
      this.students[studentIndex].tasks.push(this.taskLink);
      this.dialogViewTask = false;
    },
    editTaskLink() {
      this.students[this.viewingStudent].tasks[
        this.viewingTask
      ].taskLink = this.taskLink;
      this.dialogViewTask = false;
    },
    viewTask(studentIndex, taskIndex) {
      // use nsn instead of studentIndex for Firestore
      this.viewingStudent = studentIndex;
      this.viewingTask = taskIndex;
      this.editMode = false;
      this.taskLink = this.students[studentIndex].tasks[taskIndex].taskLink;
    },
    viewAddPassion(studentIndex) {
      this.dialogPassion = true;
      this.viewingStudent = studentIndex;
      this.editModePassion = false;
    },
    saveEditPassion(studentIndex) {
      this.students[studentIndex].passionProject = this.passionProject;
      this.dialogPassion = false;
    },
  },
};
</script>

<style lang="scss" scoped>
.profilePic {
  width: 50px;
  height: 50px;
  object-fit: cover;
  border-radius: 50% !important;
}

td {
  padding: 5px !important;
  // border: solid 1px red;

  .box {
    display: flex;
    border: solid 1px rgba(0, 0, 0, 0.6);

    .boxSides {
      flex: 1;
      text-align: center;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 0.6rem;
      padding: 5px 10px;
    }

    .borderLeft {
      border-left: solid 1px rgba(0, 0, 0, 0.6);
    }
  }
}
</style>
