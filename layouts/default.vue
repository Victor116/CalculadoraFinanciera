<template>
  <div>
    <nav
      class="navbar header has-shadow is-primary"
      role="navigation"
      aria-label="main navigation">
      <div class="navbar-brand">
        <a
          class="navbar-item"
          href="/">
          <img
            src="~assets/buefy.png"
            alt="Buefy"
            height="28">
        </a>

        <div class="navbar-burger">
          <span/>
          <span/>
          <span/>
        </div>
      </div>
    </nav>

    <section class="main-content columns">

      <aside class="column is-2 section">
        <p class="menu-label is-hidden-touch">General</p>
        <ul class="menu-list">
          <li
            v-for="(item, key) of items"
            :key="key">
            <nuxt-link
              :to="item.to"
              exact-active-class="is-active">
              <b-icon class="fas" :icon="item.icon"/> {{ item.title }}
            </nuxt-link>
          </li>
        </ul>
      </aside>

      <div class="container column is-10">
        <nuxt />
      </div>

    </section>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [
        { title: 'Pago Unico', icon: 'home', to: { name: 'index' } },
        { title: 'Anualidad Uniforme', icon: '', to: { name: 'anualidadUniforme' } },
        { title: 'Gradiente aritmetico', icon: 'chart-bar', to: { name: 'aritmetico' } },
        { title: 'Gradiente geometrico', icon: 'chart-line', to: { name: 'geometrico' } }
        
      ]
    }
  }
}
</script>
