<template>
  <div>
    <div class="container">
      <div class="left-block" :class="{'open': open}">
        <div class="box">
          <div class="form">
            <h2 class="title">Добавление товара</h2>
            <adding-product @added-products="addedProducts"/>
            <button class="side-button" :class="{'open': open}" @click="opened()">
              <span class="triangle"></span>
            </button>
          </div>
        </div>
      </div>
      <div class="right-block">
        <div class="sorting-block">
          <selected :chosen="selected" :sorting-list="list" @selected-option="sortByPrice"/>
        </div>
        <div class="content">
          <product-card v-for="(product, item) in products" @remove-product="removeProduct" :item="item" :data="product" :key="product"/>
        </div>
      </div>
      <div class="bg-gray" :class="{'open': open}" @click="open = !open"></div>
    </div>
  </div>
</template>

<script>
import AddingProduct from '../components/ProductAdditionForm.vue'
import ProductCard from '../components/ProductCard.vue'
import Selected from '../components/Select.vue'
export default {
  name: 'Home',
  components: {
    AddingProduct,
    ProductCard,
    Selected
  },
  data () {
    return {
      open: false,
      products: null,
      list: [
        { name: 'По умолчанию' },
        { name: 'min' },
        { name: 'max' }
      ],
      selected: 'По умолчанию',
      sortedProducts: []
    }
  },
  created () {
    // здесь я подгружаю товар из LocalStorage
    const arr = localStorage.getItem('products')
    this.products = JSON.parse(arr)
  },
  methods: {
    // Добавление продукта в массив и LocalStorage
    addedProducts (data) {
      this.products.push(data[data.length - 1])
      localStorage.setItem('products', JSON.stringify(this.products))
    },
    // удаление продукта из массива и localStorage
    removeProduct (data) {
      this.products.splice(data, 1)
      localStorage.setItem('products', JSON.stringify(this.products))
    },
    // удаление скрола при открытие боковой панели
    opened () {
      this.open = !this.open
      if (this.open) {
        document.querySelector('html').style.overflow = 'hidden'
      } else {
        document.querySelector('html').style.overflow = 'auto'
      }
    },
    // Метод сортировки товар по цене
    sorting (arr, option) {
      if (option === 'max') {
        arr.sort((a, b) => +a.price < +b.price ? 1 : -1)
      } else if (option === 'min') {
        arr.sort((a, b) => +a.price > +b.price ? 1 : -1)
      }
    },
    // Здесь я передую параметры для сортировки
    sortByPrice (option) {
      this.sorting(this.products, option)
    }
  },
  watch: {
    products () {
      console.log('products')
    }
  }
}
</script>

<style lang="scss">
  .container {
    width: 1376px;
    padding: 32px 0;
    margin: auto;
    display: flex;
    .bg-gray {
      display: none;
      opacity: 0;
      top: 0;
      left: 0;
      position: fixed;
      width: 100%;
      height: 100vh;
      background: #3F3F3F;
      z-index: 20;
      transition: opacity 0.3s ease-in-out;
      &.open {
        display: block;
        opacity: 0.7;
        transition: opacity 0.3s ease-in-out;
      }
    }
    .left-block {
      width: 25.5%;
      .box {
        padding-right: 16px;
        .form {
          overflow-y: auto;
          height: 100%;
        }
        .side-button {
            position: absolute;
            top: 50px;
            right: -30px;
            padding: 23px 10px;
            border-radius: 0 6px 6px 0;
            border: none;
            background: #3F3F3F;
            display: none;
            .triangle {
              display: inline-block;
              width: 12px;
              height: 12px;
              border: 2px solid #fff;
              border-bottom: 0;
              border-left: 0;
              transition: 0.3s ease-in-out;
              transform: rotate(45deg);
            }
            &.open {
              .triangle {
                transition: 0.3s ease-in-out;
                transform: rotate(225deg);
              }
            }
        }
      }
      .title {
        font-weight: 600;
        font-size: 28px;
        color: #3F3F3F;
        margin-bottom: 16px;
      }
    }
    .right-block {
      width: 74.5%;
      .sorting-block {
        display: flex;
        justify-content: flex-end;
      }
      .content {
        width: 100%;
        display: flex;
        flex-wrap: wrap;
      }
    }
  }
  @media screen and (max-width: 1440px){
    .container {
      width: 100%;
      padding: 32px;
    }
  }
  @media screen and (max-width: 1200px) {
    .container {
      .left-block {
        width: 35%;
      }
      .right-block {
        width: 65%;
        .content {
          .card {
            width: calc(50% - 8px);
            &:nth-child(3n) {
              margin-right: 16px;
            }
            &:nth-child(2n) {
              margin-right: 0;
            }
          }
        }
      }
    }
  }
  @media screen and (max-width: 912px) {
    .container {
      position: relative;
      .left-block {
        width: 400px;
        position: absolute;
        z-index: 99;
        background: #FFFEFB;
        height: 100vh;
        top: 0%;
        transition: left 0.3s ease-in-out;
        left: -400px;
        box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02);
        &.open {
          transition: left 0.3s ease-in-out;
          left: 0%;
        }
        .box {
          padding: 32px 0 32px 32px;
          position: relative;
          height: 100%;
          .side-button {
            display: block;
          }
          .product-addition-form {
            box-shadow: none;
            padding-left: 0;
          }
        }
      }
      .right-block {
        width: 100%;
      }
    }
  }
  @media screen and (max-width: 480px) {
    .container {
      .left-block {
        width: 335px;
        left: -335px;
        padding-left: 15px;
        .title {
            font-size: 22px;
        }
      }
      .right-block {
        .content {
          .card {
            width: 100%;
            margin-right: 0;
          }
        }
      }
    }
  }
  @media screen and (max-width: 365px) {
    .container {
      .left-block {
        width: 286px;
        left: -286px;
      }
    }
  }
</style>
