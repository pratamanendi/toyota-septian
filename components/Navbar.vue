<template>
  <div>
    <b-navbar
      toggleable="lg"
      fixed="top"
      :variant="width < 768 ? 'dark' : spos > 20 ? 'dark' : 'transparent'"
      type="dark"
      style="transition: all .2s ease"
      :class="spos > 20 ? 'shadow' : ''"
    >
      <b-container>
        <b-navbar-brand href="#">
          <div
            class="brand-wrapper d-flex align-items-center justify-content-between"
          >
            <b-img
              src="../assets/logo.png"
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
            <div v-for="(l, i) in link" :key="i">
              <b-nav-item @click="scroll(l.id)">{{ l.name }}</b-nav-item>
            </div>
          </b-navbar-nav>
        </b-collapse>
      </b-container>
    </b-navbar>
  </div>
</template>

<script>
export default {
  props: ['spos', 'link', 'width'],
  methods: {
    scroll(args) {
      this.$emit('scroll', args)
    }
  }
}
</script>

<style></style>
