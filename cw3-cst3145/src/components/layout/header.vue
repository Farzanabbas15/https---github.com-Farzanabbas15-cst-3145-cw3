<template>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
        <div class="container">
        <a href="javascript:void(0)" class="d-flex navbar-brand align-items-center" v-on:click="navigateTo('products')">
            <img style="height: 45px;" src="/logo.png" alt="Logo" class="me-2">
            <span> Study Hub</span>
        </a>
          <div class="d-flex">
            <div class="navbar-cart">
                
                <button v-on:click="navigateTo('checkout')" class="btn btn-danger rounded-pill">
                    <span class="pr-3">Checkout</span>
                    <span class="rounded-circle  position-relative">
                        <i class="fas fa-shopping-cart"></i>
                            {{ cart.length }}
                    </span>
                </button>
            </div>
            <div class="navbar-checkout">
                
            </div>
          </div>
        </div>
    </nav>
      
</template>
<script>
export default {
    name: 'Header',
    props: {
        page: {
            type: String,
            required: true
        },
        cart: {
            type: Array,
            required: true
        }
    },
    methods: {
        handleCheckoutClick: function() {
            if(!this.cart.length){
                Swal.fire(
                'Cart is Empty!',
                'Please Select a lesson',
                'warning'
                )
            }
        },
        navigateTo(page) {
            if(this.page === 'checkout'){
                this.$emit('navigate-to', 'products')
                return;
            }
            if(page === 'checkout' && !this.cart.length){
                Swal.fire(
                'Cart is Empty!',
                'Please Select a lesson',
                'warning'
                )
                return;
            }else{
                this.$emit('navigate-to', page)
            }
        },

    }
}

</script>