<template>  
<div class="container py-5"> 
    <div class="checkout row">
        <div class="col-md-6">
            <div class="container">
                <div class="checkout">
                    <div class="col-50">
                        <h3 class="mb-4">Billing Address</h3>
                        <div class="form-group">
                            <label for="fname"><i class="fa fa-user"></i> Full Name</label>
                            <input class="form-control" type="text" id="fname" v-model="order.name" name="firstname">
                        </div>
                        <div class="form-group">
                            <label for="email"><i class="fa fa-envelope"></i> Email</label>
                            <input class="form-control" type="text" id="email" name="email" v-model="order.email">
                        </div>
                        <div class="form-group">
                            <label for="adr"><i class="fa fa-address-card-o"></i> Address</label>
                            <input class="form-control" type="text" id="adr" v-model="order.address" name="address">
                        </div>
                        <div class="form-group">
                            <label for="city"><i class="fa fa-institution"></i> City</label>
                            <input class="form-control" type="text" v-model="order.city" id="city" name="city">
                        </div>
                        
                        <div class="form-group">
                            <div class="row">
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label for="state">State</label>
                                        <select v-model="order.state" class="form-select">
                                            <option disabled value="State">Select State</option>
                                            <option v-for="(state,key) in states" v-bind:value="state">{{key}}
                                            </option>
                                        </select>
                                    </div>
                                </div>
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label for="zip">Zip</label>
                                        <input class="form-control" v-model.number="order.zip" type="text" id="zip" name="zip">
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="form-group">
                            <button class="btn btn-primary" v-on:click="onSubmitCheckout()">Checkout</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="col-md-6">
            <div class="user-details card card-body mb-4 py-5">
                <h4>User Information
                    <span class="price" style="color:black">
                        <i class="fa-solid fa-user"></i>
                    </span>
                </h4>
                <p><a>Full Name: {{order.name}}</a></p>
                <p><a>Email Address: {{order.email}}</a></p>
                <p><a>Address: {{order.address}}</a></p>
                <p><a>City: {{order.city}}</a></p>
                <p><a>State: {{order.state}}</a></p>
                <p><a>Zip: {{order.zip}}</a></p>
                <hr>
            </div>
            <div class="cart-details card card-body mb-4">
                <h4>Cart
                        <span class="price" style="color:black">
                            <i class="fa fa-shopping-cart"></i>
                            <b>{{cart.length}}</b>
                        </span>
                </h4>

                    <ul class="list-group mb-3">
                        <li v-for="product in cart" :key="product.name" class="list-group-item d-flex justify-content-between lh-condensed">
                            <div>
                                {{ product.id }}
                                <p class="my-0">{{ product.subject }}</p> 
                                <small class="text-muted">Quantity: {{ product.cartquantity }}</small>
                            </div> 
                            <span class="text-muted">{{product.price}}</span> 
                            <div>
                                <button class="btn btn-sm btn-danger" v-on:click="removeFromCart(product)">
                                    <i class="fa-solid fa-trash mr-3"></i>
                                </button>
                            </div>
                        </li>
                    </ul>
                
                <hr>
                <p>Total <span class="price" style="color:black"><b>{{cartTotal}}</b></span></p>
            </div>
        </div>
    </div>
</div>
</template>
<script>
    export default {
        name: 'CheckoutForm',
        props: {
            cart: Array,
        },
        data: () => {
            return {
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
                    AUH: 'Abu Dhabi',
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
            onSubmitCheckout: function () {
                    if (this.order.name && this.order.email && this.order.address && this.order.city && this.order.zip && this.order.state) {
                        this.checkout.push(this.order);
                        this.$emit('discard-cart');
                        this.order = {
                                name: '',
                                email: '',
                                address: '',
                                city: '',
                                zip: '',
                                state: '',
                                method: 'Home',
                                gift: false,
                        };
                        Swal.fire(
                        'Order Places!',
                        'Your Order Has Been Placed!',
                        'success'
                        )
                        
                    } else {
                        Swal.fire(
                        'Form Not Complete!',
                        'Please Fill All the Feilds!',
                        'error'
                        )
                    }
            },
            removeFromCart: function (product) {
                this.$emit('remove-item', product);
            },
        },
        computed: {
            cartTotal() {
                let total = 0;
                this.cart.forEach((item) => {
                    total += item.price * item.cartquantity;
                });
                return total;
            }
        }
    }
</script>