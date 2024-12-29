<template>
  <div id="main">
    <a href="http://192.168.1.67:3000/" class="titlewebid2">Hepsiorada</a>
    <h1>Sepetim</h1>
    <button class="clear" @click="clearCart">Sepeti Temizle</button>
    <hr>
    <ul v-if="cart.length">
      <li v-for="(product, index) in cart" :key="index">
        <img :src="product.img" alt="Ürün Resmi" />
        <h3>{{ product.title }}</h3>
        <!-- Fiyat, ürün fiyatı ile quantity'yi çarparak hesaplanıyor -->
        <p>Fiyat: {{ product.price * product.quantity }} TL</p>

        <div id="featurecontainer">
          <p class="feature">Özellikler</p>
          <hr>
          <ul>
            <li v-for="(spec, i) in product.ozellik" :key="i">
              {{ spec }}
            </li>
          </ul>
          <div>
            <button class="arttir"@click="changeQuantity(index, 'increase')">Arttır</button>
            <button class="azalt" @click="changeQuantity(index, 'decrease')" :disabled="product.quantity <= 1">Azalt</button>
            <button @click="removeFromCart(index)">Ürünü Kaldır</button>
          </div>
        </div>
      </li>
    </ul>
    <p v-else>Sepetinizde ürün bulunmamaktadır.</p>
  </div>
</template>

<script>
import "@/pages/styles/sepet.css"

export default {
  data() {
    return {
      cart: []
    };
  },
  mounted() {
    // Sepet verilerini al ve varsa her ürüne quantity ekle
    this.cart = JSON.parse(localStorage.getItem("cart")) || [];

    this.cart.forEach(product => {
      if (!product.quantity) {
        product.quantity = 1;  // Eğer quantity yoksa, 1 olarak başlat
      }
    });
  },
  methods: {
    // Sepet ürününü kaldır
    removeFromCart(index) {
      this.cart.splice(index, 1); // Seçilen ürünü kaldır
      localStorage.setItem("cart", JSON.stringify(this.cart)); // localStorage'ı güncelle
      alert("Ürün sepetinizden kaldırıldı!");
    },

    changeQuantity(index, action) {
      const product = this.cart[index];

      if (action === 'increase') {
        product.quantity++; 

      } else if (action === 'decrease' && product.quantity > 1) {
        product.quantity--; 
      }

      localStorage.setItem("cart", JSON.stringify(this.cart));
    },

    clearCart() {
      localStorage.clear();    
      let cart = [];
      localStorage.setItem("cart", JSON.stringify(cart));
      console.log("Sepet temizlendi!");
      alert("Sepetiniz Temizlendi !")
    }
  }
}
</script>
