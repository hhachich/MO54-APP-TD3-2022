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
            <b-card-text> Catégorie : {{ item.categorie }} </b-card-text>
            <b-card-text> Prix : {{ item.prix }} </b-card-text>
            <b-button variant="dark" v-on:click="addItem(item)"
              >Ajouter au panier
            </b-button>
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
            <b-card-text> Catégorie : {{ pan.categorie }} </b-card-text>
            <b-card-text> Prix : {{ pan.prix }} </b-card-text>
            <b-button variant="danger" v-on:click="deleteItem(pan)"
              >Supprimer du panier</b-button
            >
          </b-card>
        </div>
      </b-card-group>
      <h3>Total : {{ totalPrixPanier }} €</h3>
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
      totalPrixPanier: 0,
      affichePanier: false,
      categories: [
        { id: 1, cat: "Amortisseur" },
        { id: 2, cat: "Bougie" },
        { id: 3, cat: "Essuie-glaces" },
        { id: 4, cat: "Frein" },
        { id: 5, cat: "Led" },
        { id: 6, cat: "Levier de vitesse" },
        { id: 7, cat: "Miroir" },
        { id: 8, cat: "Moteur" },
        { id: 9, cat: "Pare-chocs" },
        { id: 10, cat: "Pédale" },
        { id: 11, cat: "Porte" },
        { id: 12, cat: "Roue" },
        { id: 13, cat: "Siège" },
        { id: 14, cat: "Volant" },
      ],
    };
  },
  async created() {
    try {
      // const res = await axios.get(`http://localhost:3000/items/`);
      const res =
        await axios.get(`https://my-json-server.typicode.com/hhachich/JsonAPI-PieceGarage/items
       `);
      this.items = res.data;
      this.affichePanier = false;
      console.log(res.data);
    } catch (error) {
      console.log(error);
    }
  },
  methods: {
    addItem(item) {
      if (
        confirm("Voulez-vous ajouter à votre panier cet article :\n" + item.name )
      ) {
        console.log(item.prix);
        this.panier.push(item);
        this.totalPrixPanier =
          parseInt(this.totalPrixPanier) + parseInt(item.prix);
      }
    },
    deleteItem(item) {
      if (
        confirm("Voulez-vous supprimer cet article :\n" + item.name )
      ) {
        console.log(item);
        this.panier.splice(this.panier.indexOf(item), 1);
        this.totalPrixPanier =
          parseInt(this.totalPrixPanier) - parseInt(item.prix);
      }
    },
    async findByCategorie(categorie) {
      try {
        // const res = await axios.get(
        //   "http://localhost:3000/items?categorie=" + categorie + ""
        const res = await axios.get(
          "https://my-json-server.typicode.com/hhachich/JsonAPI-PieceGarage/items?categorie=" +
            categorie +
            ""
        );
        this.affichePanier = false;
        this.items = res.data;
      } catch (error) {
        console.log(error);
      }
    },
    async findAll() {
      try {
        // const res = await axios.get("http://localhost:3000/items");
        const res = await axios.get(
          "https://my-json-server.typicode.com/hhachich/JsonAPI-PieceGarage/items"
        );
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