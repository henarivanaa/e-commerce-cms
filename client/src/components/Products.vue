<template>
  <div class="limiter">
    <div class="container-table100">
      <div class="table">
        <div class="row header">
          <div class="cell">
            Name
          </div>
          <div class="cell">
            Image Url
          </div>
          <div class="cell">
            Price
          </div>
          <div class="cell">
            Stock
          </div>
          <div class="cell">
            Action
          </div>
        </div>

        <div v-for="product in allProducts" :key="product.id" class="row">
          <div class="cell">
            <div class="label" :class="{ hide: isEdited }">{{product.name}}</div>
            <input class="input" :class="{ show: isEdited }" type="text" v-model="name">
          </div>
          <div class="cell">
            <div class="label" :class="{ hide: isEdited }">{{product.image_url}}</div>
            <input class="input" :class="{ show: isEdited }" type="text" v-model="image_url">
          </div>
          <div class="cell">
            <div class="label" :class="{ hide: isEdited }">{{product.price}}</div>
            <input class="input" :class="{ show: isEdited }" type="number" v-model="price">
          </div>
          <div class="cell">
            <span @click.prevent="onAdd(product.id)"><i class="fas fa-angle-up"></i></span>
            {{product.stock}}
            <span @click.prevent="onSubtract(product.id)"><i class="fas fa-angle-down"></i></span>
          </div>
          <div class="cell">
            <a @click.prevent="onDelete(product.id)"><i class="fa fa-trash"></i></a>
            <router-link :to="{ name: 'Edit', params: { product, id: product.id } }">
              <span><i class="fas fa-edit"></i></span>
            </router-link>
          </div>
        </div>
      </div>
    </div>
    <slot />
  </div>
  <!-- </div> -->
</template>

<script>
import { mapGetters, mapActions } from 'vuex';

export default {
  name: 'ProductCard',
  created() {
    this.getAllProducts();
  },
  computed: mapGetters(['allProducts']),
  data() {
    return {
      name: '',
      image_url: '',
      price: 0,
      stock: 0,
      isEdited: false,
    };
  },
  methods: {
    ...mapActions(['getAllProducts', 'deleteProduct', 'editProduct']),

    onDelete(id) {
      this.deleteProduct(id);
    },

    onAdd(id) {
      const product = this.allProducts;
      const editedProduct = product.filter((produk) => produk.id === id);
      editedProduct[0].stock += 1;
      this.editProduct(editedProduct[0]);
    },

    onSubtract(id) {
      const product = this.allProducts;
      const editedProduct = product.filter((produk) => produk.id === id);
      editedProduct[0].stock -= 1;
      this.editProduct(editedProduct[0]);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  a {
    text-decoration: none;
    color: #555555;
  }

  .label.hide {
    display: none;
  }

  .input {
    display: none;
  }

  .input.show {
    display: table-cell;
  }

  span {
    cursor: pointer;
  }

  .fas {
    margin: 0px 10px;
  }

  * {
    margin: 0px;
    padding: 0px;
    box-sizing: border-box;
  }

  body, html {
    height: 100%;
  }

  /*//////////////////////////////////////////////////////////////////
  [ Table ]*/
  .limiter {
    max-height: 80vh;
    width: 100%;
    margin: 0 auto;
    overflow: auto;
  }

  .container-table100 {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-wrap: wrap;
    padding: 0px 0px;
  }

  .wrap-table100 {
    width: 960px;
    border-radius: 10px;
    overflow: hidden;
  }

  .table {
    width: 100%;
    display: table;
    margin: 0;
  }

  @media screen and (max-width: 768px) {
    .table {
      display: block;
    }
  }

  .row {
    display: table-row;
    background: #fff;
  }

  .row.header {
    color: #ffffff;
    background: #6c7ae0;
  }

  @media screen and (max-width: 768px) {
    .row {
      display: block;
    }

    .row.header {
      padding: 0;
      height: 0px;
    }

    .row.header .cell {
      display: none;
    }

    .row .cell:before {
      font-size: 12px;
      color: #808080;
      line-height: 1.2;
      text-transform: uppercase;
      font-weight: unset !important;

      margin-bottom: 13px;
      content: attr(data-title);
      min-width: 98px;
      display: block;
    }
  }

  .cell {
    display: table-cell;
  }

  @media screen and (max-width: 768px) {
    .cell {
      display: block;
    }
  }

  .row .cell {
    font-size: 15px;
    color: #666666;
    line-height: 1.2;
    font-weight: unset !important;

    padding-top: 20px;
    padding-bottom: 20px;
    border-bottom: 1px solid #f2f2f2;
  }

  .row.header .cell {
    font-size: 18px;
    color: #fff;
    line-height: 1.2;
    font-weight: unset !important;

    padding-top: 19px;
    padding-bottom: 19px;
  }

  .row .cell:nth-child(1) {
    width: 160px;
    max-width: 160px;
    overflow: auto;
  }

  .row .cell:nth-child(2) {
    width: 360px;
    max-width: 500px;
    overflow: auto;
  }

  .row .cell:nth-child(3) {
    width: 250px;
  }

  .row .cell:nth-child(4) {
    width: 90px;
  }

  .row .cell:nth-child(5) {
    width: 100px;
  }


  .table, .row {
    width: 100% !important;
  }

  .row:hover {
    background-color: #ececff;
    cursor: pointer;
  }

  @media (max-width: 768px) {
    .row {
      border-bottom: 1px solid #f2f2f2;
      padding-bottom: 18px;
      padding-top: 30px;
      padding-right: 15px;
      margin: 0;
    }

    .row .cell {
      border: none;
      padding-left: 30px;
      padding-top: 16px;
      padding-bottom: 16px;
    }
    .row .cell:nth-child(1) {
      padding-left: 30px;
    }

    .row .cell {
      font-size: 18px;
      color: #555555;
      line-height: 1.2;
      font-weight: unset !important;
    }

    .table, .row, .cell {
      width: 100% !important;
    }
  }
</style>
