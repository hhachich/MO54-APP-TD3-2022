<template >
  <div>
    <!-- ********* navbar ************* -->
    <div>
      <b-navbar type="dark" variant="dark">
        <b-navbar-brand href="#">AutoPiece</b-navbar-brand>
        <b-navbar-nav>
          <b-nav-item v-on:click="findAll()">Acceuil</b-nav-item>

          <b-nav-item-dropdown text="Catégorie" right>
            <b-dropdown-item
              v-for="categorie in categories"
              :key="categorie.id"
              v-on:click="findByCategorie(categorie.cat)"
              >{{ categorie.cat }}</b-dropdown-item
            >
          </b-nav-item-dropdown>
        </b-navbar-nav>

        <b-navbar-nav class="ml-auto">
          <b-nav-item v-on:click="ShowPanier(true)"
            >Panier: {{ panier.length }}
          </b-nav-item>
          
          <b-nav-item-dropdown text="Lang" right>
            <b-dropdown-item href="#">FR</b-dropdown-item>
            <b-dropdown-item href="#">EN</b-dropdown-item>
          </b-nav-item-dropdown>

          <b-nav-item-dropdown text="Utilisateur" right>
            <b-dropdown-item href="#">compte</b-dropdown-item>
            <b-dropdown-item href="#">paramètre</b-dropdown-item>
          </b-nav-item-dropdown>
        </b-navbar-nav>
      </b-navbar>
    </div>

    <!-- ********** Articles à la une ************ -->
    <div v-if="affichePanier == false">
      <h1>Articles à la une</h1>
      <b-card-group>
        <div v-for="item in items" :key="item.id">
          <b-card
            img-alt="Image"
            img-top
            tag="article"
            style="max-width: 20rem"
            class="mb-2"
            header-bg-variant="primary"
            align="center"
          >
            <b-card-header>
              {{ item.name }}
            </b-card-header>
            <b-card-img-lazy :src="item.img" height="200"></b-card-img-lazy>
            <b-card-text> Categorie : {{ item.categorie }} </b-card-text>
            <b-card-text> Prix : {{ item.prix }} </b-card-text>
            <b-button variant="dark" v-on:click="addItem(item)"
              >Ajouter au panier</b-button
            >
          </b-card>
        </div>
      </b-card-group>
    </div>

    <!-- *********** Panier *********** -->
    <div v-if="affichePanier == true">
      <h1>Panier</h1>
      <b-card-group>
        <div v-for="pan in panier" :key="pan.id">
          <b-card
            img-alt="Image"
            img-top
            tag="article"
            style="max-width: 20rem"
            class="mb-2"
            header-bg-variant="primary"
            align="center"
          >
            <b-card-header>
              {{ pan.name }}
            </b-card-header>
            <b-card-img-lazy :src="pan.img" height="200"></b-card-img-lazy>
            <b-card-text> Categorie : {{ pan.categorie }} </b-card-text>
            <b-card-text> Prix : {{ pan.prix }} </b-card-text>
            <b-button variant="danger" v-on:click="deleteItem(pan)"
              >Supprimer du panier</b-button
            >
          </b-card>
        </div>
      </b-card-group>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "Acceuil",
  data() {
    return {
      items: [],
      panier: [],
      affichePanier: false,
      categories: [
        { id: 1, cat: "amortisseur" },
        { id: 2, cat: "bougie" },
        { id: 3, cat: "essuie-glaces"},
        { id: 4, cat: "frein" },
        { id: 5, cat: "led" },
        { id: 6, cat: "levier de vitesse" },
        { id: 7, cat: "miroire" },
        { id: 8, cat: "pare-chocs" },
        { id: 9, cat: "pedale" },
        { id: 10, cat: "porte" },
        { id: 11, cat: "roue" },
        { id: 12, cat: "siege" },
        { id: 13, cat: "volant" },
      ],
    };
  },
  async created() {
    try {
      const res = await axios.get(`http://localhost:3000/items/`);
      this.items = res.data;
      this.affichePanier = false;
      console.log(res.data);
    } catch (error) {
      console.log(error);
    }
  },
  methods: {
    addItem(item) {
      console.log(item);
      this.panier.push(item);
    },
    deleteItem(item) {
      console.log(item);
      this.panier.splice(this.panier.indexOf(item), 1);
    },
    async findByCategorie(categorie) {
      try {
        const res = await axios.get(
          "http://localhost:3000/items?categorie=" + categorie + ""
        );
        this.affichePanier = false;
        this.items = res.data;
      } catch (error) {
        console.log(error);
      }
    },
    async findAll() {
      try {
        const res = await axios.get("http://localhost:3000/items");
        this.affichePanier = false;
        this.items = res.data;
      } catch (error) {
        console.log(error);
      }
    },
    async ShowPanier(etat) {
      this.affichePanier = etat;
    },
  },
};
</script>
<style>

.ml-auto {
  margin-left: auto;
}
</style>