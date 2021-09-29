<template>
  <form>
    <div class="product-addition-form">
      <div class="content">
        <label for="product-name" class="label">Наименование товара <span></span></label>
        <input type="text" required  id="product-name" class="input" :class="{'error': validation.invalid.name}" @focus="clearValidation('name')" v-model="product.name" placeholder="Введите наименование товара">
        <span class="error-text" v-if="validation.invalid.name">Поле является обязательным</span>
      </div>
      <div class="content">
        <label for="product-description" class="label">Описание товара</label>
        <textarea name="" id="product-description" rows="6" class="input" v-model="product.description" placeholder="Введите описание товара"></textarea>
      </div>
      <div class="content">
        <label for="product-picture" class="label">Ссылка на изображение товара<span></span></label>
        <input type="text" required  id="product-picture" class="input" :class="{'error': validation.invalid.image}" @focus="clearValidation('image')" v-model="product.image" placeholder="Введите ссылку">
        <span class="error-text" v-if="validation.invalid.image">Поле является обязательным</span>
      </div>
      <div class="content">
        <label for="product-price" class="label">Цена товара<span></span></label>
        <input type="number" required  id="product-price" class="input" :class="{'error': validation.invalid.price}" @focus="clearValidation('price')" v-model="product.price" placeholder="Введите цену">
        <span class="error-text" v-if="validation.invalid.price">Поле является обязательным</span>
      </div>
      <button class="add-product" type="button" @click="createdProduct()">Добавить товар</button>
    </div>
  </form>
</template>

<script>
export default {
  data () {
    return {
      products: [],
      product: {
        name: 'Наименование товара',
        description: 'Довольно-таки интересное описание товара в несколько строк. Довольно-таки интересное описание товара в несколько строк',
        image: 'https://thumbs.dreamstime.com/b/cosmos-beauty-deep-space-elements-image-furnished-nasa-science-fiction-art-102581846.jpg',
        price: '10000'
      },
      validation: {
        invalid: {

        },
        valid: {

        }
      }
    }
  },
  methods: {
    createdProduct () {
      if (this.product.name !== '' && this.product.image !== '' && this.product.price !== '') {
        this.products.push({
          name: this.product.name,
          description: this.product.description,
          image: this.product.image,
          price: this.product.price
        })
      } else {
        if (!this.product.name) {
          this.validation.invalid.name = true
        }
        if (!this.product.image) {
          this.validation.invalid.image = true
        }
        if (!this.product.price) {
          this.validation.invalid.price = true
        }
        this.$forceUpdate()
      }
    },
    clearValidation (field) {
      this.validation.invalid[field] = false
      this.$forceUpdate()
    }
  },
  watch: {
    products: {
      handler (val) {
        this.$emit('added-products', val)
      },
      deep: true
    }
  }
}
</script>

<style scoped lang="scss">
  .product-addition-form {
    background: #FFFEFB;
    padding: 24px;
    border-radius: 4px;
    box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02);
    .content {
      width: 100%;
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      padding-bottom: 16px;
      position: relative;
      &:nth-child(4){
        margin-bottom: 24px;
      }
      .label {
        font-size: 10px;
        color: #49485E;
        margin-bottom: 4px;
        cursor: pointer;
        position: relative;
        span {
          position: absolute;
          width: 4px;
          height: 4px;
          border-radius: 100%;
          background: #FF8484;
          top: 0;
          right: -7px;
        }
      }
      .input {
        padding: 10px 0 10px 16px;
        width: 100%;
        box-sizing: border-box;
        outline: none;
        border-radius: 4px;
        border: 1px solid transparent;
        box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
        font-size: 12px;
        color: #3F3F3F;
        &::-webkit-input-placeholder {
          color: #B4B4B4;
        }
        &.error {
          border: 1px solid #FF8484;
        }
      }
      .error-text {
        position: absolute;
        bottom: 2px;
        left: 0;
        font-size: 8px;
        color: #FF8484;
      }
    }
    .add-product {
      width: 100%;
      cursor: pointer;
      font-size: 12px;
      font-weight: 600;
      color: #FFFFFF;
      background: #7BAE73;
      padding: 10px 0;
      box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
      border: none;
      border-radius: 10px;
      text-align: center;
    }
  }
</style>
