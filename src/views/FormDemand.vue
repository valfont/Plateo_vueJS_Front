<template>
  <div id="mainContainer">
    <!-- <div v-show="showModal" class="modal"> -->
    <form
      v-show="showModal"
      id="modal"
      class="form-style-9"
      @submit.prevent="createNewDemand()"
    >
      <a class="align-right" href="/">Annuler</a>
      <h3>Création d'une nouvelle demande</h3>
      <ul>
        <fieldset>
          <legend>
            <input
              type="text"
              name="field1"
              placeholder="Titre de la demande"
              class="field-style field-full"
              v-model="title"
              style="text-transform: uppercase"
            />
          </legend>
          <li>
            <select class="field-style field-full" v-model="clients_id">
              <option value="">Attribuée à :</option>
              <option
                value="1"
                selected
                :key="key"
                v-for="(el, key) in clients"
              >
                <p>{{ el.nameRS }}</p>
                <p>{{ el.lastname }}</p>
              </option>
            </select>
          </li>

          <li>
            <select id="statut" v-model="status" class="field-style field-full">
              <option value="">Choix du statut</option>
              <option selected value="1 - Attente acceptation artisan">
                1 - Attente acceptation artisan
              </option>
              <option value="2 - Attente de réception devis">
                2 - Attente de réception devis
              </option>
              <option value="3 - Attente d'approbation devis">
                3 - Attente d'approbation devis
              </option>
              <option value="4 - Attente de devis">4 - Attente de devis</option>
              <option value="5 - Travaux à réaliser">
                5 - Travaux à réaliser
              </option>
              <option value="6 - Travaux réalisés">6 - Travaux réalisés</option>
              <option value="7 - Travaux annulés">7 - Travaux annulés</option>
              <option value="8 - Demande annulée">8 - Demande annulée</option>
            </select>
          </li>
          <li>
            <input
              type="text"
              name="field4"
              class="field-style field-full"
              placeholder="Adresse des Travaux"
              v-model="adresse"
            />
          </li>

          <li>
            <input
              type="date"
              name="field3"
              class="field-style field-split align-left"
              placeholder="Date début travaux"
              v-model="start"
            />
            <input
              type="date"
              name="field4"
              class="field-style field-split align-right"
              placeholder="Date fin travaux"
              v-model="end"
            />
          </li>
          <li>
            <textarea
              type="text"
              name="field4"
              class="field-style field-full"
              placeholder="Description"
              v-model="description"
            />
          </li>
        </fieldset>
      </ul>

      <br />

      <input type="submit" value="Enregistrer" class="submit" />
    </form>
  </div>
  <div v-show="showModal" class="overlay"></div>
  <!-- </div> -->
</template>

<script>
import axios from "axios";
export default {
  name: "FormDemand",
  components: {},
  data: function () {
    return {
      // id: "",
      clients: [],
      title: "",
      description: "",
      adresse: "", //adresse des travaux
      clients_id: "",
      start: "",
      end: "",
      status: "",
      showModal: true,
      newCustomer: true,
    };
  },
  async mounted() {
    axios
      .get("http://127.0.0.1:8000/api/client")
      .then((response) => (this.clients = response.data));
  },
  methods: {
    toggleModal() {
      this.showModal = !this.showModal;
    },

    toggleCustomerExist(param) {
      if (param === "exist") {
        this.newCustomer = true;
      } else {
        this.newCustomer = false;
      }
    },
    createNewDemand: async function () {
      const body = {
        title: this.title,
        description: this.description,
        adresse: this.adresse, //adresse des travaux
        clients_id: this.clients_id,
        start: this.start,
        end: this.end,
        status: this.status,
      };
      const response = await axios.post(
        "http://127.0.0.1:8000/api/demande",
        body
      );

      if (response) {
        this.$router.push("/");
      } else {
        alert("Votre requête n'a pas été prise en compte");
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
fieldset {
  border-radius: 5px;
  padding: 20px;
  border: #083d77 1px solid;
}

legend {
  padding: 5px;
  color: #083d77;
}
.submit {
  border-radius: 5px;
  box-shadow: #f8f8f8 5px 5px 5px;
  padding: 1em;
  background-color: #2b90d9;
  color: white;
  border: 0;
  font-weight: bold;
}
html,
body {
  margin: 0;
}

#mainContainer .modal {
  position: absolute;
  top: 30px;
  left: 30px;
  right: 30px;
  bottom: 30px;
  background-color: white;
  box-shadow: 3px 4px 8px black;
  border-radius: 5px;
  padding: 20px;
  z-index: 10;
}
#mainContainer .overlay {
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  background-color: black;
  opacity: 0.7;
}
.align-right {
  float: right;
  text-align: right;
  margin-right: 0;
}
.form-style-9 {
  max-width: 450px;
  background: #fafafa;
  padding: 30px;
  margin: 10px auto;
  box-shadow: 1px 1px 25px rgba(0, 0, 0, 0.35);
  border-radius: 10px;
  border: 6px solid #2b90d9;
}
.form-style-9 ul {
  padding: 0;
  margin: 0;
  list-style: none;
}
.form-style-9 ul li {
  display: block;
  margin-bottom: 10px;
  min-height: 35px;
}
.form-style-9 ul li .field-style {
  box-sizing: border-box;
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  padding: 8px;
  outline: none;
}
.form-style-9 ul li .field-style:focus {
  box-shadow: 0 0 5px #b0cfe0;
  border: 1px solid #b0cfe0;
}
.form-style-9 ul li .field-split {
  width: 49%;
}
.form-style-9 ul li .field-full {
  width: 100%;
}
.form-style-9 ul li input.align-left {
  float: left;
}
.form-style-9 ul li input.align-right {
  float: right;
}
.form-style-9 ul li textarea {
  width: 100%;
  height: 100px;
}
.form-style-9 ul li input[type="button"],
.form-style-9 ul li input[type="submit"] {
  -moz-box-shadow: inset 0px 1px 0px 0px #3985b1;
  -webkit-box-shadow: inset 0px 1px 0px 0px #3985b1;
  box-shadow: inset 0px 1px 0px 0px #3985b1;
  background-color: #2b90d9;
  border: 1px solid #083d77;
  display: inline-block;
  cursor: pointer;
  color: #ffffff;
  padding: 8px 18px;
  text-decoration: none;
  font: 12px Arial, Helvetica, sans-serif;
  border-radius: 5px;
}
.form-style-9 ul li input[type="button"]:hover,
.form-style-9 ul li input[type="submit"]:hover {
  background: linear-gradient(to bottom, #2d77a2 5%, #337da8 100%);
  background-color: #2b90d9;
  border-radius: 5px;
}
fieldset {
  border-radius: 5px;
  padding: 20px;
  border: #2b90d9 1px solid;
}

legend {
  padding: 5px;
  color: #2b90d9;
}
.submit {
  border-radius: 5px;
  box-shadow: #f8f8f8 5px 5px 5px;
  padding: 1em;
  background-color: #2b90d9;
  color: white;
  border: 0;
  font-weight: bold;
}
.form-style-9 {
  max-width: 550px;
  padding: 30px;
  margin: 10px auto;
}
.form-style-9 ul {
  padding: 0;
  margin: 0;
  list-style: none;
}
.form-style-9 ul li {
  display: block;
  margin-bottom: 10px;
  min-height: 35px;
}
.form-style-9 ul li .field-style {
  box-sizing: border-box;
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  padding: 8px;
  outline: none;
}
.form-style-9 ul li .field-style:focus {
  box-shadow: 0 0 5px #b0cfe0;
  border: 1px solid #b0cfe0;
}
.form-style-9 ul li .field-split {
  width: 49%;
}
.form-style-9 ul li .field-full {
  width: 100%;
}
.form-style-9 ul li input.align-left {
  float: left;
}
.form-style-9 ul li input.align-right {
  float: right;
}
.form-style-9 ul li textarea {
  width: 100%;
  height: 100px;
}
.form-style-9 ul li input[type="button"],
.form-style-9 ul li input[type="submit"] {
  -moz-box-shadow: inset 0px 1px 0px 0px #3985b1;
  -webkit-box-shadow: inset 0px 1px 0px 0px #3985b1;
  box-shadow: inset 0px 1px 0px 0px #3985b1;
  background-color: #2b90d9;
  border: 1px solid #083d77;
  display: inline-block;
  cursor: pointer;
  color: #ffffff;
  padding: 8px 18px;
  text-decoration: none;
  font: 12px Arial, Helvetica, sans-serif;
  border-radius: 5px;
}
.form-style-9 ul li input[type="button"]:hover,
.form-style-9 ul li input[type="submit"]:hover {
  background: linear-gradient(to bottom, #2d77a2 5%, #337da8 100%);
  background-color: #2b90d9;
  border-radius: 5px;
}
h3 {
  color: #2b90d9;
}
</style>
