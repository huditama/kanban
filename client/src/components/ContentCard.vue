<template>
  <div class="cardContent">
    <h2>{{item.title}}</h2>
    <!-- ACTION BUTTONS -->
    <div class="container-button">
      <div>
        <h4 @click="removeTask">Remove!!</h4>
      </div>

      <div>
        <h4 @click="changeNext">{{firstStage}}</h4>
      </div>

      <div>
        <h4 @click="nextStage" v-if="nextPhase">{{nextPhase}}</h4>
      </div>
    </div>
    <!-- END OF ACTION BUTTONS -->
  </div>
</template>

<script>
import database from "../assets/config.js";
import swal from "sweetalert";

export default {
  name: "ContentCard",
  props: ["item", "name"],
  data: function() {
    return {
      firstStage: "",
      nextPhase: ""
    };
  },
  methods: {
    changeNext() {
      database.ref(`/${this.item.id}`).remove();
      database.ref("/").push({
        title: this.item.title,
        status: this.firstStage
      });
    },
    nextStage() {
      database.ref(`/${this.item.id}`).remove();
      database.ref("/").push({
        title: this.item.title,
        status: this.nextPhase
      });
    },
    removeTask() {
      swal({
        title: "Are you sure?",
        text: "Once deleted, you will not be able to recover this task!",
        icon: "warning",
        buttons: true,
        dangerMode: true
      }).then(willDelete => {
        if (willDelete) {
          database.ref(`/${this.item.id}`).remove();
          swal("Poof! Task has been deleted!", {
            icon: "success"
          });
        } else {
          swal("Whew.. That was close!!");
        }
      });
    }
  },
  created() {
    if (this.name === "Backlog") {
      this.firstStage = "To-Do";
      this.nextPhase = "";
    } else if (this.name === "To-Do") {
      this.firstStage = "Backlog";
      this.nextPhase = "On-Going";
    } else if (this.name === "On-Going") {
      this.firstStage = "Finished";
      this.nextPhase = "To-Do";
    } else if (this.name === "Finished") {
      this.firstStage = "On-Going";
      this.nextPhase = "";
    }
  }
};
</script>

<style >
.cardContent {
  font-size: 15px;
  color: #fff;
  width: 90%;
  min-height: 100px;
  background-color: #4684f6;
  -webkit-box-shadow: 0px 5px 4px #c0c0c09d;
  -moz-box-shadow: 0px 5px 4px #c0c0c09d;
  box-shadow: 0px 5px 4px #c0c0c09d;
  border-radius: 5px;
  margin-left: auto;
  margin-right: auto;
  text-align: center;
  margin-bottom: 50px;
  vertical-align: middle;
  cursor: pointer;
  padding-bottom: 20px;
}

.card h2 {
  padding-top: 20px;
  font-size: 20px;
}

.container-button h4 {
  color: #4684f6;
  background-color: #fff;
  margin-left: auto;
  margin-right: auto;
  margin-top: 5px;
  text-align: center;
  width: 30%;
  padding: 2%;
  border-radius: 5px;
}

.container {
  background-color: whitesmoke !important;
  width: 100%;
  min-height: 675px;
}
</style>