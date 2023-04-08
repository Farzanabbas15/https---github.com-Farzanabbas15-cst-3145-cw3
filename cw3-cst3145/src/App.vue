<script setup>
  import HeaderComponent from '@/components/layout/header.vue'
  import ProductList from '@/components/ProductList.vue';
  import CheckoutForm from '@/components/CheckoutForm.vue';
</script>

<template>
    <HeaderComponent :page="page" :cart="cart"  v-on:navigate-to="navigateTo" v-on:checkout-cart="checkoutCart"  />

    <ProductList v-if="page == 'products'"  
                  v-bind:products="products"
                  v-bind:page="page"
                  v-on:checkout-cart="checkoutCart"
                  v-on:add-to-cart="addToCart"
    />

    <CheckoutForm v-if="page == 'checkout'"
                 v-bind:products="products"
                 v-bind:cart="cart"
                 v-bind:checkout="checkout" 
                 v-on:discard-cart="discardCart"
                 v-on:remove-item="removeFromCart"
    />
    
</template>

<script>
import {products} from '@/assets/JS/products'
export default {
  data: () => {
    return {
      page: 'products',
      cart: [],
      sortBy: 'subject',
      sortDirection: 'asc',
      products: products,
      checkout: [],
      order: {
        name: '',
        email: '',
        address: '',
        city: '',
        zip: '',
        state: '',
        method: 'Home',
        gift: false,
      },
      states: {
        SIN: 'Abu Dhabi',
        AJM: 'Ajman',
        DXB: 'Dubai',
        FUJ: 'Fujairah',
        RAK: 'Ras Al Khaimah',
        SHJ: 'Sharjah',
        UMM: 'Umm Al Quwain',
      },
    };

  },
  methods: {
    addToCart(product) {
      if (!this.cart.includes(product)) {
        this.cart.push(product);
        product.cartquantity++;
      }
      else
      product.cartquantity++;

      Swal.fire('Cart Updated!',( 'You have Added ' + product.subject + ' to Your Cart!'),'success')
      
      // this.products.forEach((item) => {
      //   if (item.id === product.id) {
      //     item.space -= 1;
      //   }
      // })
    },
    removeFromCart(product) {
      
      let item = this.cart.indexOf(product);
      this.cart.splice(item,1);

      let LessonObj = this.products.find(l => l.id === product.id);
      LessonObj.space += product.cartquantity;
      LessonObj.cartquantity = 0;

      //alert when item is removed from cart
      Swal.fire('Removed from Cart!',(product.subject + ' has been removed from your cart!'), 'warning');
     

      //navigate to products page if cart is empty
      if(!this.cart.length) {
        this.navigateTo("products")
      }
    },
    navigateTo(page) {
      this.page = page;
    },
    checkoutCart() {
      if (this.cart.length > 0) {
        this.page = "checkout";
      } else {
        Swal.fire(
          'Cart is Empty!',
          'Please Select a lesson',
          'warning'
        )
      }
    },
    discardCart() {
      
      this.cart = [];
      this.page = "products";
    },
  },
  computed: {
    cartCount() {
      let count = 0;
      this.cart.forEach((item) => {
        count += item.cartquantity;
      });
      return count;
    },
  },
}
</script>

