<template>
    <div>
        <div class="background-container">
            <div class="page-container">
                <div class="container">
                    <div class="product-container">
                        <div v-if="loading">
                            <div class="loader"></div>
                        </div>
                        <div v-else>
                            <div class="product-item" v-if="Object.keys(product).length > 0">
                                <div class="product-content">
                                    <div class="image-container" v-if="product.title !== 'Product not available'">
                                        <img :src="product.image" alt="Product Image" class="product-image">
                                    </div>
                                    <div class="text-container">
                                        <div class="title-container">
                                            <h2
                                                :class="{ 'blue-text': product.category === 'men\'s clothing', 'pink-text': product.category === 'women\'s clothing' }">
                                                {{ product.title }}
                                            </h2>
                                            <div class="category-rating">
                                                <p v-if="product.category === 'men\'s clothing'" class="category-text">
                                                    Men's Clothing
                                                </p>
                                                <p v-if="product.category === 'women\'s clothing'"
                                                    class="category-text">
                                                    Women's Clothing
                                                </p>
                                                <div class="rating" v-if="product.title !== 'Product not available'"
                                                    :class="{ 'blue-star': product.category === 'men\'s clothing', 'pink-star': product.category === 'women\'s clothing' }">
                                                    ★★★★☆
                                                </div>

                                                <hr v-if="product.title !== 'Product not available'"
                                                    class="category-rating">
                                            </div>
                                        </div>
                                        <hr v-if="product.title !== 'Product not available'" class="category-rating">
                                        <p v-if="product.title !== 'Product not available'">{{ product.description }}
                                        </p>
                                        <hr v-if="product.title !== 'Product not available'">
                                        <p class="price"
                                            :class="{ 'blue-text': product.category === 'men\'s clothing', 'pink-text': product.category === 'women\'s clothing' }"
                                            v-if="product.title !== 'Product not available'">
                                            ${{ product.price }}
                                        </p>
                                        <div class="button-container">
                                            <button
                                                v-if="Object.keys(product).length > 0 && product.title !== 'Product not available'"
                                                class="buy-button"
                                                :class="{ 'pink-button': product.category === 'women\'s clothing' }">Buy
                                                Now</button>

                                            <button @click="fetchProduct" v-if="Object.keys(product).length > 0"
                                                class="next-button"
                                                :class="{ 'pink2-button': product.category === 'women\'s clothing' }">Next
                                                Product</button>
                                        </div>
                                    </div>
                                </div>
                                <div class="product-background" v-if="product.title === 'Product not available'"></div>
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
    name: 'ProductDisplay',
    data() {
        return {
            product: {},
            loading: false,
            index: 1
        };
    },
    methods: {
        async fetchProduct() {
            try {
                this.loading = true;
                const response = await fetch(`https://fakestoreapi.com/products/${this.index}`);
                const data = await response.json();
                if (data.category === "men's clothing" || data.category === "women's clothing") {
                    this.product = data;
                    document.querySelector('.page-container').classList.remove('mens-clothing', 'womens-clothing');
                    document.querySelector('.page-container').classList.add(data.category === "men's clothing" ? 'mens-clothing' : 'womens-clothing');
                    document.querySelector('.page-container').classList.remove('not-available');
                } else {
                    this.product = { title: 'Product not available', description: '', price: '' };
                    document.querySelector('.page-container').classList.add('not-available');
                }
                this.index = (this.index % 20) + 1;
            } catch (error) {
                console.error('Error fetching product:', error);
            } finally {
                this.loading = false;
            }
        }
    },
    mounted() {
        this.fetchProduct();
    }
};
</script>

<style src="./style.css"></style>