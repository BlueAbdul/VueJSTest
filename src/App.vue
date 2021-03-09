// Template
<template>
  <div id="app">
    <div class="container">
      <div class="card p-5">
        <h3>Editer une facture</h3>
        <hr />
        <div class="row">
          <div class="col-4">
            <label for="Facture">Facture N° : </label>
            <input
              type="number"
              name="facture"
              id="facture"
              v-model="bill.id"
            />
            <br /><br />
            <label for="emission">Emise le : </label>
            <input
              type="date"
              name="emission"
              id="emission"
              v-model="bill.date"
            />
            <br /><br />
            <label for="client">Client :</label>
            <input type="list" v-model="fullname" />
          </div>
          <div class="col-8">
            <label for="desc">Description :</label>
            <input
              style="width:70%;"
              type="text"
              name="desc"
              id="desc"
              v-model="bill.description"
            />
            <br /><br />
            <label for="emissiondesc">Emise le </label>
            <input type="date" name="emissiondesc" id="emissiondesc" v-model="bill.date"/>
            <br /><br />
            <input type="checkbox" name="TVA" id="" v-model="bill.tva" />
            <label for="TVA">Soumise TVA 20%</label>
          </div>
        </div>
        <br /><br />
        <div class="row">
          <table>
            <thead>
              <tr>
                <th>Actions</th>
                <th style="width:50%;">Prestation</th>
                <th>Quantité</th>
                <th class="text-center">Montant unitaire</th>
                <th class="text-center">Montant total</th>
              </tr>
            </thead>
            <button v-bind:class="{ show : isEmpty}" class="btn btn-info hide" @click="add()"> Ajouter prestation </button>
            <tbody>
              <tr v-for='(bill,index) in bill.prestations' :key='index'>
                
                <td>
                  <svg
                    @click="add()"
                    xmlns="http://www.w3.org/2000/svg"
                    width="16"
                    height="16"
                    fill="currentColor"
                    class="bi bi-plus-circle"
                    viewBox="0 0 16 16"
                  >
                    <path
                      d="M8 15A7 7 0 1 1 8 1a7 7 0 0 1 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"
                    />
                    <path
                      d="M8 4a.5.5 0 0 1 .5.5v3h3a.5.5 0 0 1 0 1h-3v3a.5.5 0 0 1-1 0v-3h-3a.5.5 0 0 1 0-1h3v-3A.5.5 0 0 1 8 4z"
                    />
                  </svg>
                  <svg
                    @click="supprimer(index)"
                    xmlns="http://www.w3.org/2000/svg"
                    width="16"
                    height="16"
                    fill="red"
                    class="bi bi-trash"
                    viewBox="0 0 16 16"
                  >
                    <path
                      d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6z"
                    />
                    <path
                      fill-rule="evenodd"
                      d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1zM4.118 4L4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118zM2.5 3V2h11v1h-11z"
                    />
                  </svg>
                </td>
                <td>
                  <input type="text"  v-model="bill.description">
                </td>
                <td>
                  <input type="number"  v-model="bill.qty">
                </td>
                <td>
                  <input type="number"  v-model="bill.price">
                </td>
                 <td>
                  <input type="number"  :value="total(bill.qty,bill.price)">
                </td>
              </tr>
                <tr>
                <td colspan="3"></td>
                <td>Remise :</td>
                <td><input type="number"  v-model="bill.discount"></td>
              </tr>
              <tr>
                <td colspan="3"></td>
                <td>Déjà payé :</td>
                <td><input type="number"  v-model="bill.paid"></td>
              </tr>
              <tr>
                <td colspan="3"></td>
                <td>TVA 20% :</td>
                <td><input type="number" disabled="disabled" v-model="tva"></td>
              </tr>
              <tr>
                <td colspan="3"></td>
                <td>Total TTC :</td>
                <td><input type="number" disabled="disabled" v-model="totalTTC"></td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
//Controleur du composant

export default {
  name: "App",
  data() {
    return {
      title: "Hello world",
      bill: {
        id: 1,
        date: new Date().getDay(),
        description: "Site web",
        tva: true,
        client: {
          idclient: 1,
          firstname: "John",
          lastname: "Marston",
        },
        prestations: [
          {
            description: "Etude graphique UX",
            qty: 1,
            price: 450.0,
          },
          {
            description: "Création site wordpress",
            qty: 3,
            price: 1000.0,
          },
          {
            description: "Hébergement annuel",
            qty: 1,
            price: 50.0,
          },
        ],
        discount: 0.0,
        paid: 0.0,
      },
    };
  },
  computed: {
    fullname() {
      return `${this.bill.client.firstname} ${this.bill.client.lastname}`;
    },
    totalHT(){
      let ht = 0;
      this.bill.prestations.forEach((e) =>{
       let result =  e.qty * e.price
       ht += result
      })

      ht -= this.bill.paid
      ht -= this.bill.discount
      
      return ht;
    },
    tva(){
      let tva = 0;
      if(this.bill.tva == true){
        tva = this.totalHT * 0.2
      }
      return tva
    },
    totalTTC(){
      let totalTTC = this.totalHT + this.tva
      return totalTTC
    },
    isEmpty(){
      let empty = false
      if(this.bill.prestations.length < 1){
          empty = true
                  
      }
      return empty
     
    }
  },

  methods:{
    total(qty,price){
       let result =  qty * price
      return result
    },
    supprimer(i){
      this.bill.prestations.splice(i,1)
    },
    add(){
      let newPrest =  {
            description: "Nouvelle prestation",
            qty: 1,
            price: 450.0,
          }

       this.bill.prestations.push(newPrest);   
    }
  }
};
</script>

<style>
@import url("https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/css/bootstrap.min.css");

table {
  width: 100%;
}

th {
  border-bottom: lightgray 1px solid;
  color: lightgray;
}
label {
  min-width: 100px;
}

.hide{
  display: none;
}

.show{
  display: block;
}
</style>
