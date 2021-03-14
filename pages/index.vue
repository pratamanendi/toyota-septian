<template>
  <div class="index">
    <Navbar
      @scroll="scrollTo"
      :link="arrLink"
      :width="windowWidth"
      :spos="sPos"
    />
    <section id="head-section" class="">
      <b-container
        class="wrapper"
        style="min-height: 60vh; padding: 100px 12px"
      >
        <b-row class="align-items-center justify-content-between">
          <b-col cols="12" lg="5" class="text-white">
            <h1>
              <b class="text-danger mb-0">TOYOTA</b> <br />
              <em>Septian Aditya</em>
            </h1>
            <p class="mb-0">
              Professional Salesman - <em>Toyota Liek Motor Surabaya</em>
            </p>
            <hr style="border-top: 1px solid #fff" />
            <p>
              Wujudkan Toyota Impianmu dengan saya, karena harganya murah dan
              prosesnya mudah
            </p>
            <label for="search-input">Mobil apa yang kamu cari ?</label>
            <b-form inline @submit.prevent="search()">
              <b-input-group>
                <b-form-input
                  id="search-input"
                  class="shadow mr-2 bg-transparent text-white"
                  placeholder="Kijang Innova"
                  v-model="searchText"
                  type="text"
                  list="my-list-id"
                ></b-form-input>
                <datalist id="my-list-id">
                  <option v-for="(car, i) in cars" :key="i">{{
                    car.jenis
                  }}</option>
                </datalist>
                <b-input-group-append>
                  <b-button
                    type="submit"
                    class="shadow text-white"
                    variant="secondary"
                  >
                    <em>Go..</em>
                  </b-button>
                </b-input-group-append>
              </b-input-group>
            </b-form>
          </b-col>
          <b-col cols="12" lg="7" class="d-sm-none d-none d-lg-block">
            <b-img-lazy
              src="../assets/innova-fr-2.png"
              alt="All New Kijang Innova Liek Motor Surabaya"
              fluid
            />
          </b-col>
        </b-row>
      </b-container>
    </section>
    <!-- slide -->
    <section
      id="vehicle"
      style="min-height: 50vh; padding: 50px 0; background: #d3d3d3"
    >
      <b-container class="py-3">
        <div id="text-vehicle" class="my-4">
          <h2 class="text-danger h1 font-weight-bold">VEHICLE</h2>
          <em class="h3">Jenis Mobil</em>
        </div>
        <b-tabs
          content-class="mt-3"
          justified
          lazy
          active-nav-item-class="font-weight-bold"
        >
          <b-tab
            v-for="(t, i) in tabs"
            :key="i"
            @click="getCars(t.id_kategori)"
            title-link-class="text-dark"
            :title="t.kategori"
            style="transition: all .3s ease"
          >
            <p v-if="!katCars.length" class="text-center">
              <em>Oops. no car(s) yet</em>
            </p>
            <b-row class="py-5">
              <b-col
                cols="12"
                lg="3"
                md="6"
                sm="12"
                v-for="(k, i) in katCars"
                :key="i"
              >
                <b-card
                  :title="k.jenis"
                  title-tag="h4"
                  sub-title="Starting at"
                  :img-src="
                    require(`../assets/img/${k.img.id_jenis}/${k.img.id_warna}.png`)
                  "
                  :img-alt="`Toyota ${k.jenis} murah liek motor surabaya`"
                  img-top
                  tag="article"
                  style="border-radius: 12px;"
                  class="shadow my-2 mx-0 px-2 py-4 bg-light"
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
          </b-tab>
        </b-tabs>
      </b-container>
    </section>
  </div>
</template>

<script>
export default {
  data() {
    return {
      slide: 0,
      sliding: null,
      windowWidth: null,
      sPos: null,
      searchText: '',
      searchRes: [],
      carsName: [],
      tabs: [],
      katCars: [],
      cars: [],
      arrLink: [
        { id: 'vehicle', name: 'Vehicle' },
        { id: 'dealer', name: 'Dealer Pricelist' }
      ]
    }
  },
  methods: {
    scrollTo(args) {
      let el = document.getElementById(args)
      el.scrollIntoView({ behavior: 'smooth' })
    },
    getCars(id) {
      this.katCars = this.cars.filter(val => {
        return val.kategori == id
      })
    },
    // getImg(obj) {
    //   return `~@/assets/img/${obj.id_jenis}/${obj.id_warna}.png`
    // },
    getUri(args) {
      return `/dealer-products/toyota-${this.$slugify(
        args.id_jenis,
        args.jenis
      )}`
    },
    async setInit() {
      let tabs = await this.$axios.get('http://localhost/api_toyota/infos')
      this.tabs = tabs.data.rec.kategori
      this.cars = tabs.data.rec.jenis
      this.getCars(1)
    },
    search() {
      let searchRes = this.cars.find(val => {
        return val.jenis === this.searchText
      })
      this.$router.push(this.getUri(searchRes))
    }
    // onSlideStart(slide) {
    //   this.sliding = true;
    // },
    // onSlideEnd(slide) {
    //   this.sliding = false;
    // }
  },
  mounted() {
    this.setInit()
    this.windowWidth = window.innerWidth
    window.addEventListener('scroll', () => {
      this.sPos = window.scrollY
    })
    window.addEventListener('resize', () => {
      this.windowWidth = window.innerWidth
    })
  },
  filters: {
    toRupiah(value) {
      return new Intl.NumberFormat('id-ID', {
        style: 'currency',
        currency: 'IDR'
      }).format(value)
    }
  }
}
</script>

<style scoped>
#head-section {
  background-image: url('~@/assets/bg.jpg');
  background-size: cover;
  background-repeat: no-repeat;
}
</style>
