<template>
  <article>
    <b-navbar toggleable="lg" variant="dark" type="dark">
      <b-container>
        <b-navbar-brand href="#">
          <div
            class="brand-wrapper d-flex align-items-center justify-content-between"
          >
            <b-img
              src="../../assets/logo.png"
              fluid
              width="60px"
              height="40px"
              alt="Logo Toyota - Septian Aditya"
            />
            <p
              class="text-danger px-2 text-sm mb-0 font-weight-bold"
              style="line-height: 1"
            >
              TOYOTA <br />
              <em class="text-white font-weight-normal">Septian Aditya</em>
            </p>
          </div>
        </b-navbar-brand>
        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav class="ml-auto text-lg">
            <!-- <div v-for="(l, i) in link" :key="i"> -->
            <!-- <b-nav-item @click="scroll(l.id)">{{ l.name }}</b-nav-item> -->
            <!-- </div> -->
          </b-navbar-nav>
        </b-collapse>
      </b-container>
    </b-navbar>
    <section class="shadow" style="background: black">
      <b-container class="py-1 text-white">
        <small>
          <nuxt-link to="/" class="btn btn-sm text-white">Home</nuxt-link>
          > Dealer Products >
          {{ address }}
        </small>
      </b-container>
    </section>
    <section class="bg-secondary">
      <b-container class="py-5">
        <b-row class="justify-content-around align-items-center px-4">
          <b-col cols="12" lg="8" md="6" sm="12" class="mb-4">
            <div style="border-radius: 20px" class=" bg-light  shadow p-4">
              <div class="text-center" id="append-img">
                <b-img lazy :src="warna_img" center fluid />
                <p class="font-weight-bold h3">{{ warna_name }}</p>
              </div>
              <hr />
              <div class="flex align-items-center text-center">
                <p class="h5">Pilihan Warna</p>
                <b-img
                  lazy
                  fluid
                  width="100"
                  class="btn btn-sm"
                  v-for="(w, i) in mobil.warna"
                  :key="i"
                  @click="
                    warna_name = w.warna_name
                    warna_img = require(`../../assets/img/${w.id_jenis}/${w.id_warna}.png`)
                  "
                  :alt="address + ' ' + w.warna_name + ' murah Surabaya'"
                  :src="
                    require(`../../assets/img/${w.id_jenis}/${w.id_warna}.png`)
                  "
                />
                <p class="text-sm text-secondary">
                  <span class="text-danger">*</span> Warna diatas dapat berbeda
                  dengan warna mobil yang sesungguhnya
                </p>
              </div>
            </div>
          </b-col>

          <b-col cols="12" lg="4" md="6" sm="12">
            <div
              id="card-text"
              style="border-radius: 20px"
              class="text-center text-white bg-dark shadow p-4"
            >
              <h1 class="font-weight-bold text-danger">
                {{ address.replace('TOYOTA', '') }}
              </h1>
              <p>
                <b>
                  <small>Harga OTR Surabaya Mulai</small>
                  <br />
                  <p class="h3">
                    {{
                      mobil.harga[0].min_manual != null
                        ? mobil.harga[0].min_manual
                        : mobil.harga[0].min_automatic | toRupiah
                    }}
                  </p>
                </b>
              </p>
              <hr style="border-top: 1px solid rgba(255,255,255,.8)" />
              <div id="btn-placement">
                <p class="font-weight-bold">Hubungi Kami</p>
                <b-button
                  style="min-width: 5em"
                  class="mx-2 px-4 py-2 shadow"
                  pill
                  variant="success"
                  :href="
                    `https://api.whatsapp.com/send?phone=6285230847278&text=Saya%20tertarik%20untuk%20mengetahui%20informasi%20mobil%20${address.replace(
                      ' ',
                      '%20'
                    )}`
                  "
                  >Whatsapp</b-button
                >
                <b-button
                  style="min-width: 5em"
                  class="mx-2 px-3 py-2 shadow"
                  pill
                  variant="light"
                  href="tel:+6285230847278"
                  >Telp</b-button
                >
              </div>
            </div>
          </b-col>
        </b-row>
      </b-container>
    </section>
    <section id="varspek">
      <b-container class="py-5">
        <h3 class="font-weight-bold h2 text-danger ">PRICE LIST</h3>
        <div
          style="border-radius: 20px"
          class="text-center bg-dark text-white shadow p-4 mt-4"
        >
          <b-table
            dark
            :items="mobil.tipe"
            :fields="fields"
            :borderless="windowWidth > 768"
            responsive="sm"
            :small="windowWidth < 768"
            :stripped="windowWidth < 768"
          >
            <template #cell(tipe)="data">
              {{ data.item.tipe }}
            </template>
            <template #cell(manual)="data">
              {{ data.item.manual | toRupiah }}
            </template>
            <template #cell(automatic)="data">
              {{ data.item.automatic | toRupiah }}
            </template>
            <template #table-caption
              >Harga Sesuai OTR Surabaya dan Dapat Berubah
              Sewaktu-waktu</template
            >
          </b-table>
        </div>
      </b-container>
    </section>
    <section id="rekom" class="bg-secondary">
      <b-container class="py-5">
        <div
          id="product-wrap"
          class="bg-light shadow-lg p-4"
          style="border-radius: 30px"
        >
          <div id="text-product" class="my-2 text-center">
            <h2 class="h1 font-weight-bold">PRODUK TERKAIT</h2>
          </div>
          <b-row class="py-2">
            <b-col
              cols="12"
              lg="3"
              md="6"
              sm="12"
              v-for="(k, i) in mobil.rekom"
              :key="i"
            >
              <b-card
                :title="k.jenis"
                title-tag="h4"
                sub-title="Starting at"
                :img-alt="`Toyota ${k.jenis} murah liek motor surabaya`"
                img-top
                tag="article"
                style="border-radius: 15px;"
                class="shadow my-2 mx-0 px-2 py-4 bg-white"
                :img-src="
                  require(`../../assets/img/${k.img.id_jenis}/${k.img.id_warna}.png`)
                "
              >
                <b-card-text>
                  <h5 class="font-weight-bold">
                    {{
                      k.harga[0].min_manual != null
                        ? k.harga[0].min_manual
                        : k.harga[0].min_automatic | toRupiah
                    }}
                  </h5>
                </b-card-text>
                <nuxt-link :to="getUri(k)" class="btn btn-outline-dark"
                  ><em>Explore</em>
                </nuxt-link>
              </b-card>
            </b-col>
          </b-row>
        </div>
      </b-container>
    </section>
  </article>
</template>

<script>
export default {
  async asyncData({ params, $axios }) {
    const id_jenis = params.tipe.split('-').pop()
    const mbl = await $axios.$post('http://localhost/api_toyota/infos/car', {
      id_jenis
    })
    const mobil = mbl.rec
    return { mobil }
  },
  data() {
    return {
      warna_img: '',
      warna_name: '',
      address: '',
      windowWidth: null,
      fields: ['tipe', 'manual', 'automatic']
    }
  },
  methods: {
    splicer() {
      var adr = this.$route.params.tipe.split('-')
      adr.splice(adr.length - 1, 1)
      var apc = adr
        .toString()
        .replace(/,/g, ' ')
        .toUpperCase()
      this.address = apc
    },
    getUri(args) {
      return (
        '/dealer-products/toyota-' + this.$slugify(args.id_jenis, args.jenis)
      )
    }
  },
  mounted() {
    this.splicer()
    let warnaMobil = this.mobil.warna[0]
    this.warna_img = require(`../../assets/img/${warnaMobil.id_jenis}/${warnaMobil.id_warna}.png`)
    this.warna_name = warnaMobil.warna_name
    this.windowWidth = window.innerWidth
  },
  filters: {
    toRupiah(value) {
      if (value == null) return '-'
      return new Intl.NumberFormat('id-ID', {
        style: 'currency',
        currency: 'IDR'
      }).format(value)
    }
  }
}
</script>

<style scoped>
/* #append-text {
  background-image: url("~@/assets/bg.jpg");
  background-size: cover;
  background-repeat: no-repeat;
} */
</style>
