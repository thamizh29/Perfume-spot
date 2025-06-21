<template>
  <div class="min-h-screen bg-gray-50">
    <!-- Header -->
    <header class="bg-white shadow-lg sticky top-0 z-50">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex justify-between items-center h-16">
          <!-- Logo -->
          <div class="flex items-center">
            <h1 class="text-2xl font-bold text-purple-600">Perfume Spot</h1>
          </div>

          <!-- Navigation -->
          <nav class="hidden md:flex space-x-8">
            <a href="#" class="text-gray-700 hover:text-purple-600 transition-colors">Home</a>
            <a href="#" class="text-gray-700 hover:text-purple-600 transition-colors">Men</a>
            <a href="#" class="text-gray-700 hover:text-purple-600 transition-colors">Women</a>
            <a href="#" class="text-gray-700 hover:text-purple-600 transition-colors">Brands</a>
            <a href="#" class="text-gray-700 hover:text-purple-600 transition-colors">About</a>
          </nav>

          <!-- Cart and Mobile Menu -->
          <div class="flex items-center space-x-4">
            <button @click="toggleCart" class="relative p-2 text-gray-700 hover:text-purple-600 transition-colors">
              <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                  d="M3 3h2l.4 2M7 13h10l4-8H5.4m0 0L7 13m0 0l-2.5 5M7 13l2.5 5m6-5v6a2 2 0 01-2 2H9a2 2 0 01-2-2v-6m8 0V9a2 2 0 00-2-2H9a2 2 0 00-2 2v4.01">
                </path>
              </svg>
              <span v-if="cartItemsCount > 0"
                class="absolute -top-1 -right-1 bg-purple-600 text-white text-xs rounded-full h-5 w-5 flex items-center justify-center">
                {{ cartItemsCount }}
              </span>
            </button>

            <!-- Mobile menu button -->
            <button @click="toggleMobileMenu" class="md:hidden p-2 text-gray-700">
              <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16">
                </path>
              </svg>
            </button>
          </div>
        </div>

        <!-- Mobile Navigation -->
        <div v-if="showMobileMenu" class="md:hidden py-4 border-t">
          <nav class="flex flex-col space-y-2">
            <a href="#" class="text-gray-700 hover:text-purple-600 py-2">Home</a>
            <a href="#" class="text-gray-700 hover:text-purple-600 py-2">Men</a>
            <a href="#" class="text-gray-700 hover:text-purple-600 py-2">Women</a>
            <a href="#" class="text-gray-700 hover:text-purple-600 py-2">Brands</a>
            <a href="#" class="text-gray-700 hover:text-purple-600 py-2">About</a>
          </nav>
        </div>
      </div>
    </header>

    <!-- Hero Section -->
    <section class="bg-gradient-to-r from-purple-600 to-pink-600 text-white py-20">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 text-center">
        <h2 class="text-4xl md:text-6xl font-bold mb-4">Discover Your Signature Scent</h2>
        <p class="text-xl md:text-2xl mb-8">Premium perfumes for every occasion</p>
        <button
          class="bg-white text-purple-600 px-8 py-3 rounded-full font-semibold hover:bg-gray-100 transition-colors">
          Shop Now
        </button>
      </div>
    </section>

    <!-- Filters -->
    <section class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
      <div class="flex flex-wrap gap-4 items-center justify-between mb-8">
        <div class="flex flex-wrap gap-4">
          <!-- Gender Filter -->
          <select v-model="selectedGender"
            class="px-8 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent">
            <option value="">All Genders</option>
            <option value="men">Men</option>
            <option value="women">Women</option>
            <option value="unisex">Unisex</option>
          </select>

          <!-- Brand Filter -->
          <select v-model="selectedBrand"
            class="px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent">
            <option value="">All Brands</option>
            <option v-for="brand in brands" :key="brand" :value="brand">
              {{ brand }}
            </option>
          </select>

          <!-- Price Filter -->
          <select v-model="selectedPriceRange"
            class="px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent">
            <option value="">All Prices</option>
            <option value="0-50">$0 - $50</option>
            <option value="50-100">$50 - $100</option>
            <option value="100-200">$100 - $200</option>
            <option value="200+">$200+</option>
          </select>
        </div>

        <!-- Search -->
        <div class="relative">
          <input v-model="searchQuery" type="text" placeholder="Search perfumes..."
            class="pl-10 pr-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-purple-500 focus:border-transparent w-64" />
          <svg class="absolute left-3 top-2.5 w-5 h-5 text-gray-400" fill="none" stroke="currentColor"
            viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
              d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path>
          </svg>
        </div>
      </div>
    </section>

    <!-- Products Grid -->
    <section class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 pb-16">
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
        <div v-for="product in filteredProducts" :key="product.id"
          class="bg-white rounded-lg shadow-md hover:shadow-xl transition-shadow duration-300 overflow-hidden group">
          <!-- Product Image -->
          <div class="relative overflow-hidden">
            <img :src="product.image" :alt="product.name"
              class="w-full h-64 object-cover group-hover:scale-105 transition-transform duration-300" />
            <div class="absolute top-4 left-4">
              <span class="bg-purple-600 text-white px-2 py-1 rounded-full text-xs font-semibold">
                {{ product.gender }}
              </span>
            </div>
            <button @click="toggleWishlist(product.id)"
              class="absolute top-4 right-4 p-2 bg-white rounded-full shadow-md hover:bg-gray-50 transition-colors">
              <svg class="w-5 h-5" :class="product.isWishlisted ? 'text-red-500 fill-current' : 'text-gray-400'
                " fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                  d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z">
                </path>
              </svg>
            </button>
          </div>

          <!-- Product Info -->
          <div class="p-4">
            <h3 class="font-semibold text-lg mb-1 text-gray-800">{{ product.name }}</h3>
            <p class="text-gray-600 text-sm mb-2">{{ product.brand }}</p>
            <p class="text-gray-500 text-sm mb-3 line-clamp-2">
              {{ product.description }}
            </p>

            <!-- Rating -->
            <div class="flex items-center mb-3">
              <div class="flex text-yellow-400">
                <svg v-for="i in 5" :key="i" class="w-4 h-4"
                  :class="i <= product.rating ? 'fill-current' : 'text-gray-300'" viewBox="0 0 20 20">
                  <path
                    d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z" />
                </svg>
              </div>
              <span class="text-gray-600 text-sm ml-2">({{ product.reviews }})</span>
            </div>

            <!-- Price and Actions -->
            <div class="flex items-center justify-between">
              <div class="flex items-center space-x-2">
                <span class="text-2xl font-bold text-purple-600">${{ product.price }}</span>
                <span v-if="product.originalPrice" class="text-gray-400 line-through">${{ product.originalPrice
                }}</span>
              </div>
            </div>

            <div class="mt-4 flex space-x-2">
              <button @click="addToCart(product)"
                class="flex-1 bg-purple-600 text-white py-2 px-4 rounded-lg hover:bg-purple-700 transition-colors font-semibold">
                Add to Cart
              </button>
              <button @click="openProductModal(product)"
                class="px-4 py-2 border border-purple-600 text-purple-600 rounded-lg hover:bg-purple-50 transition-colors">
                View
              </button>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Shopping Cart Sidebar -->
    <div v-if="showCart" class="fixed inset-0 z-50 overflow-hidden">
      <div class="absolute inset-0 bg-black bg-opacity-50" @click="toggleCart"></div>
      <div class="absolute right-0 top-0 h-full w-full max-w-md bg-white shadow-xl">
        <div class="flex flex-col h-full">
          <!-- Cart Header -->
          <div class="flex items-center justify-between p-4 border-b">
            <h2 class="text-lg font-semibold">Shopping Cart</h2>
            <button @click="toggleCart" class="p-2 hover:bg-gray-100 rounded-full">
              <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
              </svg>
            </button>
          </div>

          <!-- Cart Items -->
          <div class="flex-1 overflow-y-auto p-4">
            <div v-if="cartItems.length === 0" class="text-center text-gray-500 mt-8">
              <svg class="w-16 h-16 mx-auto mb-4 text-gray-300" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                  d="M3 3h2l.4 2M7 13h10l4-8H5.4m0 0L7 13m0 0l-2.5 5M7 13l2.5 5m6-5v6a2 2 0 01-2 2H9a2 2 0 01-2-2v-6m8 0V9a2 2 0 00-2-2H9a2 2 0 00-2 2v4.01">
                </path>
              </svg>
              <p>Your cart is empty</p>
            </div>

            <div v-else class="space-y-4">
              <div v-for="item in cartItems" :key="item.id"
                class="flex items-center space-x-4 bg-gray-50 p-3 rounded-lg">
                <img :src="item.image" :alt="item.name" class="w-16 h-16 object-cover rounded" />
                <div class="flex-1">
                  <h3 class="font-semibold text-sm">{{ item.name }}</h3>
                  <p class="text-gray-600 text-xs">{{ item.brand }}</p>
                  <p class="text-purple-600 font-semibold">${{ item.price }}</p>
                </div>
                <div class="flex items-center space-x-2">
                  <button @click="updateQuantity(item.id, item.quantity - 1)" class="p-1 hover:bg-gray-200 rounded">
                    <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20 12H4"></path>
                    </svg>
                  </button>
                  <span class="w-8 text-center">{{ item.quantity }}</span>
                  <button @click="updateQuantity(item.id, item.quantity + 1)" class="p-1 hover:bg-gray-200 rounded">
                    <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                        d="M12 6v6m0 0v6m0-6h6m-6 0H6"></path>
                    </svg>
                  </button>
                </div>
                <button @click="removeFromCart(item.id)" class="p-1 text-red-500 hover:bg-red-50 rounded">
                  <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                      d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16">
                    </path>
                  </svg>
                </button>
              </div>
            </div>
          </div>

          <!-- Cart Footer -->
          <div v-if="cartItems.length > 0" class="border-t p-4">
            <div class="flex justify-between items-center mb-4">
              <span class="font-semibold">Total: ${{ cartTotal.toFixed(2) }}</span>
            </div>
            <button
              class="w-full bg-purple-600 text-white py-3 rounded-lg hover:bg-purple-700 transition-colors font-semibold">
              Checkout
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Product Modal -->
    <div v-if="selectedProduct" class="fixed inset-0 z-50 overflow-y-auto">
      <div class="flex items-center justify-center min-h-screen px-4">
        <div class="absolute inset-0 bg-black bg-opacity-50" @click="closeProductModal"></div>
        <div class="relative bg-white rounded-lg max-w-4xl w-full max-h-screen overflow-y-auto">
          <div class="grid md:grid-cols-2 gap-8 p-6">
            <!-- Product Image -->
            <div>
              <img :src="selectedProduct.image" :alt="selectedProduct.name"
                class="w-full h-96 object-cover rounded-lg" />
            </div>

            <!-- Product Details -->
            <div>
              <button @click="closeProductModal" class="absolute top-4 right-4 p-2 hover:bg-gray-100 rounded-full">
                <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                </svg>
              </button>

              <h2 class="text-3xl font-bold mb-2">{{ selectedProduct.name }}</h2>
              <p class="text-gray-600 mb-4">{{ selectedProduct.brand }}</p>

              <!-- Rating -->
              <div class="flex items-center mb-4">
                <div class="flex text-yellow-400">
                  <svg v-for="i in 5" :key="i" class="w-5 h-5" :class="i <= selectedProduct.rating ? 'fill-current' : 'text-gray-300'
                    " viewBox="0 0 20 20">
                    <path
                      d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z" />
                  </svg>
                </div>
                <span class="text-gray-600 ml-2">({{ selectedProduct.reviews }} reviews)</span>
              </div>

              <p class="text-gray-700 mb-6">{{ selectedProduct.description }}</p>

              <!-- Price -->
              <div class="flex items-center space-x-4 mb-6">
                <span class="text-3xl font-bold text-purple-600">${{ selectedProduct.price }}</span>
                <span v-if="selectedProduct.originalPrice" class="text-gray-400 line-through text-xl">${{
                  selectedProduct.originalPrice }}</span>
              </div>

              <!-- Size Selection -->
              <div class="mb-6">
                <h3 class="font-semibold mb-2">Size</h3>
                <div class="flex space-x-2">
                  <button v-for="size in selectedProduct.sizes" :key="size"
                    class="px-4 py-2 border border-gray-300 rounded-lg hover:border-purple-600 hover:text-purple-600 transition-colors">
                    {{ size }}
                  </button>
                </div>
              </div>

              <!-- Actions -->
              <div class="flex space-x-4">
                <button @click="addToCart(selectedProduct)"
                  class="flex-1 bg-purple-600 text-white py-3 px-6 rounded-lg hover:bg-purple-700 transition-colors font-semibold">
                  Add to Cart
                </button>
                <button @click="toggleWishlist(selectedProduct.id)"
                  class="px-6 py-3 border border-purple-600 text-purple-600 rounded-lg hover:bg-purple-50 transition-colors">
                  <svg class="w-5 h-5" :class="selectedProduct.isWishlisted ? 'fill-current' : ''" fill="none"
                    stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                      d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z">
                    </path>
                  </svg>
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Footer -->
    <footer class="bg-gray-800 text-white py-12">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="grid grid-cols-1 md:grid-cols-4 gap-8">
          <div>
            <h3 class="text-xl font-bold mb-4">Perfume Spot</h3>
            <p class="text-gray-300">
              Your destination for premium fragrances from around the world.
            </p>
          </div>
          <div>
            <h4 class="font-semibold mb-4">Quick Links</h4>
            <ul class="space-y-2 text-gray-300">
              <li><a href="#" class="hover:text-white transition-colors">About Us</a></li>
              <li><a href="#" class="hover:text-white transition-colors">Contact</a></li>
              <li><a href="#" class="hover:text-white transition-colors">Shipping</a></li>
              <li><a href="#" class="hover:text-white transition-colors">Returns</a></li>
            </ul>
          </div>
          <div>
            <h4 class="font-semibold mb-4">Categories</h4>
            <ul class="space-y-2 text-gray-300">
              <li>
                <a href="#" class="hover:text-white transition-colors">Men's Fragrances</a>
              </li>
              <li>
                <a href="#" class="hover:text-white transition-colors">Women's Fragrances</a>
              </li>
              <li><a href="#" class="hover:text-white transition-colors">Unisex</a></li>
              <li>
                <a href="#" class="hover:text-white transition-colors">Gift Sets</a>
              </li>
            </ul>
          </div>
          <div>
            <h4 class="font-semibold mb-4">Follow Us</h4>
            <div class="flex space-x-4">
              <a href="#" class="text-gray-300 hover:text-white transition-colors">
                <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24">
                  <path
                    d="M24 4.557c-.883.392-1.832.656-2.828.775 1.017-.609 1.798-1.574 2.165-2.724-.951.564-2.005.974-3.127 1.195-.897-.957-2.178-1.555-3.594-1.555-3.179 0-5.515 2.966-4.797 6.045-4.091-.205-7.719-2.165-10.148-5.144-1.29 2.213-.669 5.108 1.523 6.574-.806-.026-1.566-.247-2.229-.616-.054 2.281 1.581 4.415 3.949 4.89-.693.188-1.452.232-2.224.084.626 1.956 2.444 3.379 4.6 3.419-2.07 1.623-4.678 2.348-7.29 2.04 2.179 1.397 4.768 2.212 7.548 2.212 9.142 0 14.307-7.721 13.995-14.646.962-.695 1.797-1.562 2.457-2.549z" />
                </svg>
              </a>
              <a href="#" class="text-gray-300 hover:text-white transition-colors">
                <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24">
                  <path
                    d="M22.46 6c-.77.35-1.6.58-2.46.69.88-.53 1.56-1.37 1.88-2.38-.83.5-1.75.85-2.72 1.05C18.37 4.5 17.26 4 16 4c-2.35 0-4.27 1.92-4.27 4.29 0 .34.04.67.11.98C8.28 9.09 5.11 7.38 3 4.79c-.37.63-.58 1.37-.58 2.15 0 1.49.75 2.81 1.91 3.56-.71 0-1.37-.2-1.95-.5v.03c0 2.08 1.48 3.82 3.44 4.21a4.22 4.22 0 0 1-1.93.07 4.28 4.28 0 0 0 4 2.98 8.521 8.521 0 0 1-5.33 1.84c-.34 0-.68-.02-1.02-.06C3.44 20.29 5.7 21 8.12 21 16 21 20.33 14.46 20.33 8.79c0-.19 0-.37-.01-.56.84-.6 1.56-1.36 2.14-2.23z" />
                </svg>
              </a>
              <a href="#" class="text-gray-300 hover:text-white transition-colors">
                <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24">
                  <path
                    d="M12.017 0C5.396 0 .029 5.367.029 11.987c0 5.079 3.158 9.417 7.618 11.174-.105-.949-.199-2.403.041-3.439.219-.937 1.406-5.957 1.406-5.957s-.359-.72-.359-1.781c0-1.663.967-2.911 2.168-2.911 1.024 0 1.518.769 1.518 1.688 0 1.029-.653 2.567-.992 3.992-.285 1.193.6 2.165 1.775 2.165 2.128 0 3.768-2.245 3.768-5.487 0-2.861-2.063-4.869-5.008-4.869-3.41 0-5.409 2.562-5.409 5.199 0 1.033.394 2.143.889 2.741.099.12.112.225.085.345-.09.375-.293 1.199-.334 1.363-.053.225-.172.271-.402.165-1.495-.69-2.433-2.878-2.433-4.646 0-3.776 2.748-7.252 7.92-7.252 4.158 0 7.392 2.967 7.392 6.923 0 4.135-2.607 7.462-6.233 7.462-1.214 0-2.357-.629-2.75-1.378l-.748 2.853c-.271 1.043-1.002 2.35-1.492 3.146C9.57 23.812 10.763 24.009 12.017 24.009c6.624 0 11.99-5.367 11.99-11.988C24.007 5.367 18.641.001.012.001z" />
                </svg>
              </a>
            </div>
          </div>
        </div>
        <div class="border-t border-gray-700 mt-8 pt-8 text-center text-gray-300">
          <p>&copy; 2024 Perfume Spot. All rights reserved.</p>
        </div>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";
import { BlackImage, BlueImage, BlueTwoImage, GreenImage, GreenTwoImage, GreenThreeImage, OrangeImage, ViloteImage, PurpleImage, PurpleTwoImage, SandleImage, RoseImage } from "../utils/imageConfig";

// Reactive data
const showMobileMenu = ref(false);
const showCart = ref(false);
const selectedProduct = ref(null);
const searchQuery = ref("");
const selectedGender = ref("");
const selectedBrand = ref("");
const selectedPriceRange = ref("");
const cartItems = ref([]);

// Sample products data
const products = ref([
  {
    id: 1,
    name: "Chanel No. 5",
    brand: "Chanel",
    price: 150,
    originalPrice: 180,
    image: BlackImage,
    description: "The world's most iconic fragrance. A timeless floral aldehyde.",
    gender: "women",
    rating: 5,
    reviews: 1250,
    sizes: ["30ml", "50ml", "100ml"],
    isWishlisted: false,
  },
  {
    id: 2,
    name: "Dior Sauvage",
    brand: "Dior",
    price: 120,
    image: BlueImage,
    description: "A fresh and woody fragrance with bergamot and pepper.",
    gender: "men",
    rating: 4,
    reviews: 890,
    sizes: ["60ml", "100ml"],
    isWishlisted: false,
  },
  {
    id: 3,
    name: "Tom Ford Black Orchid",
    brand: "Tom Ford",
    price: 200,
    image: BlueTwoImage,
    description: "A luxurious and sensual fragrance with dark florals.",
    gender: "unisex",
    rating: 5,
    reviews: 567,
    sizes: ["50ml", "100ml"],
    isWishlisted: false,
  },
  {
    id: 4,
    name: "Versace Bright Crystal",
    brand: "Versace",
    price: 80,
    originalPrice: 95,
    image: PurpleTwoImage,
    description: "A fresh and floral fragrance with pomegranate and peony.",
    gender: "women",
    rating: 4,
    reviews: 723,
    sizes: ["30ml", "90ml"],
    isWishlisted: false,
  },
  {
    id: 5,
    name: "Creed Aventus",
    brand: "Creed",
    price: 350,
    image: PurpleImage,
    description: "A sophisticated blend of pineapple, birch, and musk.",
    gender: "men",
    rating: 5,
    reviews: 1456,
    sizes: ["50ml", "100ml", "120ml"],
    isWishlisted: false,
  },
  {
    id: 6,
    name: "Yves Saint Laurent Black Opium",
    brand: "YSL",
    price: 110,
    image: OrangeImage,
    description: "An addictive gourmand fragrance with coffee and vanilla.",
    gender: "women",
    rating: 4,
    reviews: 934,
    sizes: ["30ml", "50ml", "90ml"],
    isWishlisted: false,
  },
  {
    id: 7,
    name: "Bleu de Chanel",
    brand: "Chanel",
    price: 140,
    image: RoseImage,
    description: "A woody aromatic fragrance with citrus and cedar.",
    gender: "men",
    rating: 5,
    reviews: 1123,
    sizes: ["50ml", "100ml", "150ml"],
    isWishlisted: false,
  },
  {
    id: 8,
    name: "Marc Jacobs Daisy",
    brand: "Marc Jacobs",
    price: 75,
    originalPrice: 90,
    image: SandleImage,
    description: "A fresh and feminine fragrance with wild berries and jasmine.",
    gender: "women",
    rating: 4,
    reviews: 678,
    sizes: ["30ml", "50ml", "100ml"],
    isWishlisted: false,
  },
    {
    id: 9,
    name: "Creed Aventus",
    brand: "Creed",
    price: 350,
    image: GreenImage,
    description: "A sophisticated blend of pineapple, birch, and musk.",
    gender: "men",
    rating: 5,
    reviews: 1456,
    sizes: ["50ml", "100ml", "120ml"],
    isWishlisted: false,
  },
  {
    id: 10,
    name: "Yves Saint Laurent Black Opium",
    brand: "YSL",
    price: 110,
    image: GreenTwoImage,
    description: "An addictive gourmand fragrance with coffee and vanilla.",
    gender: "women",
    rating: 4,
    reviews: 934,
    sizes: ["30ml", "50ml", "90ml"],
    isWishlisted: false,
  },
  {
    id: 11,
    name: "Bleu de Chanel",
    brand: "Chanel",
    price: 140,
    image: GreenThreeImage,
    description: "A woody aromatic fragrance with citrus and cedar.",
    gender: "men",
    rating: 5,
    reviews: 1123,
    sizes: ["50ml", "100ml", "150ml"],
    isWishlisted: false,
  },
  {
    id: 12,
    name: "Marc Jacobs Daisy",
    brand: "Marc Jacobs",
    price: 75,
    originalPrice: 90,
    image: ViloteImage,
    description: "A fresh and feminine fragrance with wild berries and jasmine.",
    gender: "women",
    rating: 4,
    reviews: 678,
    sizes: ["30ml", "50ml", "100ml"],
    isWishlisted: false,
  },
]);

// Computed properties
const brands = computed(() => {
  return [...new Set(products.value.map((p) => p.brand))].sort();
});

const filteredProducts = computed(() => {
  return products.value.filter((product) => {
    const matchesSearch =
      product.name.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
      product.brand.toLowerCase().includes(searchQuery.value.toLowerCase());
    const matchesGender =
      !selectedGender.value || product.gender === selectedGender.value;
    const matchesBrand = !selectedBrand.value || product.brand === selectedBrand.value;
    const matchesPrice =
      !selectedPriceRange.value ||
      checkPriceRange(product.price, selectedPriceRange.value);

    return matchesSearch && matchesGender && matchesBrand && matchesPrice;
  });
});

const cartItemsCount = computed(() => {
  return cartItems.value.reduce((total, item) => total + item.quantity, 0);
});

const cartTotal = computed(() => {
  return cartItems.value.reduce((total, item) => total + item.price * item.quantity, 0);
});

// Methods
const toggleMobileMenu = () => {
  showMobileMenu.value = !showMobileMenu.value;
};

const toggleCart = () => {
  showCart.value = !showCart.value;
};

const openProductModal = (product) => {
  selectedProduct.value = product;
};

const closeProductModal = () => {
  selectedProduct.value = null;
};

const addToCart = (product) => {
  const existingItem = cartItems.value.find((item) => item.id === product.id);

  if (existingItem) {
    existingItem.quantity += 1;
  } else {
    cartItems.value.push({
      ...product,
      quantity: 1,
    });
  }

  // Show success message (you can implement a toast notification here)
  console.log(`${product.name} added to cart!`);
};

const removeFromCart = (productId) => {
  cartItems.value = cartItems.value.filter((item) => item.id !== productId);
};

const updateQuantity = (productId, newQuantity) => {
  if (newQuantity <= 0) {
    removeFromCart(productId);
    return;
  }

  const item = cartItems.value.find((item) => item.id === productId);
  if (item) {
    item.quantity = newQuantity;
  }
};

const toggleWishlist = (productId) => {
  const product = products.value.find((p) => p.id === productId);
  if (product) {
    product.isWishlisted = !product.isWishlisted;
  }
};

const checkPriceRange = (price, range) => {
  switch (range) {
    case "0-50":
      return price >= 0 && price <= 50;
    case "50-100":
      return price > 50 && price <= 100;
    case "100-200":
      return price > 100 && price <= 200;
    case "200+":
      return price > 200;
    default:
      return true;
  }
};

// Lifecycle
onMounted(() => {
  // Initialize any data or perform setup
  console.log("Perfume Spot loaded!");
});
</script>

<style scoped>
.line-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
</style>
