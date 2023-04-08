<template>
    <div class="container">
    <div class="product-filters py-5 row justify-content-between">
        <div class="col-md-4">
            <div class="form-group">
                <label for="searchTerm">Search</label>
                <input class="form-control-lg" type="text" placeholder="type search term here.. ." name="searchTerm" id="searchTerm" v-model="searchTerm">
            </div>
        </div>
        <div class="col-md-6">
            <div class="row">
                <div class="col-md-6">
                    <div class="form-group">
                        <label for="sortDir">Sort Direction</label>
                        <select id="sortDir" name="sortDir" class="form-select" v-model="sortDirection">
                            <option value="asc">Ascending</option>
                            <option value="desc">Descending</option>
                        </select>
                    </div>
                </div>
                <div class="col-md-6">
                    <div class="form-group" style="margin-left: 5%;">
                        <label for="exampleFormControlSelect1">Filter By</label>
                        <select class="form-select" v-model="sortBy">
                            <option value="subject"> Alphabatically</option>
                            <option value="price">Price</option>
                            <option value="space">Availability</option>
                            <option value="location">Location</option>
                        </select>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div class="product-list">
        <div class="lesssonbox row">
            <div class="col-md-3" v-for="product in filteredProducts">
                <div class="card mb-4">
                    <div class="card-body p-0">
                        <div class="product-image">
                            <img :src="product.image" class="w-100">
                        </div>
                        <div class="p-3">
                            <div class="product-details">
                                <h4 style="margin-top:15px;">{{product.subject}}</h4>
                                <h6 class="mt-0 text-black-50">{{product.location}}</h6>
                            </div>
                            <div class="stats mt-2">
                                <div class="d-flex justify-content-between p-price">
                                    <span>Available Spaces</span>
                                    <span>{{product.space}}</span>
                                </div>
                            </div>
                            <div class="d-flex justify-content-between p-price">
                                <span>Price </span>
                                <span>{{product.price}}</span>
                            </div>
                            <div class="d-flex justify-content-between total font-weight-bold mt-4">
                                <button class="btn btn-primary rounded w-100" v-on:click="addToCart(product)" v-if="spaceCount(product)">Add to Cart</button>
                                <button v-else disabled>Add to Cart</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
</template>

<script>
 export default {
        name: 'ProductList',
        props: {
           products: {
               type: Array,
               required: true
           },
        },
        data() {
            return {
                sortBy: 'subject',
                sortDirection: 'asc',
                searchTerm: ''
            }
        },
        methods: {
            checkoutCart() {
                this.$emit('checkout-cart');
            },
            spaceCount(product) {
                if (product.space > 0) {
                    return true;
                } else {
                    return false;
                }
            },
            addToCart(product) {
                this.$emit('add-to-cart', product);
                this.products.forEach((item) => {
                    if (item.id === product.id) {
                        item.space--;
                    }
                });
            }
        },
        computed: {
            filteredProducts() {
                let filterLes = this.products;

                //filter by search term
                if(this.searchTerm !== '' && this.searchTerm){
                    filterLes = filterLes.filter((product) => {
                       return product.subject.toLowerCase().includes(this.searchTerm.toLowerCase()) || product.location.toLowerCase().includes(this.searchTerm.toLowerCase());
                    })
                }

                //sort by direction
                if(this.sortBy === 'subject') {
                    filterLes =  filterLes.sort((a, b) => {
                        if (this.sortDirection === 'asc') {
                            return a.subject.localeCompare(b.subject);
                        } else if (this.sortDirection === 'desc') {
                            return b.subject.localeCompare(a.subject);
                        }
                    })
                }
                //sort by price 
                if (this.sortBy === 'price') {
                    filterLes = filterLes.sort((a, b) => {
                    if (this.sortDirection === 'asc') {
                        return a.price - b.price;
                    } else if (this.sortDirection === 'desc') {
                        return b.price - a.price;
                    }
                    });
                }

                //sort by space
                if (this.sortBy === 'space') {
                    filterLes = filterLes.sort((a, b) => {
                    if (this.sortDirection === 'asc') {
                        return a.space - b.space;
                    } else if (this.sortDirection === 'desc') {
                        return b.space - a.space;
                    }
                    });
                }

                //sort by location
                if (this.sortBy === 'location') {
                    filterLes = filterLes.sort((a, b) => {
                    if (this.sortDirection === 'asc') {
                        console.log(a.location, a.location, a.location < b.location ? -1 : a.location > b.location ? 1 : 0);
                       
                        return  a.location < b.location ? -1 : a.location > b.location ? 1 : 0;
                    } else if (this.sortDirection === 'desc') {
                        console.log(a.location, b.location, a.location < b.location ? -1 : a.location > b.location ? 1 : 0);
                        return b.location < a.location ? -1 : b.location > a.location ? 1 : 0;
                    }
                    });
                }
                
                return filterLes;
            },  
        }
 }

</script>