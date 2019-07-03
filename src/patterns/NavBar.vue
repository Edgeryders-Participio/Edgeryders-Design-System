<template>
  <component :is="type" class="nav">
    <Logo size="large" fill="white" variation="A" />
    <h2>{{ title }}</h2>

    <ul class="menu">
      <li v-for="(item, index) in navItems">
        <a
          v-if="item.href"
          :key="index"
          :href="item.href"
          :class="{ active: localActive === item.component }"
          v-html="item.name"
          v-smooth-scroll="{ duration: 1000, offset: -100 }"
        />
        <div class="dropdown" v-if="item.dropdown">
          <button
            class="dropbtn"
            @click="
              isactive = !isactive
              activedropdown = item.dropdown
            "
          >
            {{ item.name }}
          </button>
          <div class="dropdown-content" :class="{ active: isactive }">
            <a v-for="link in item.dropdown" :href="link[1]" target="_blank">{{ link[0] }}</a>
          </div>
        </div>
      </li>
    </ul>
    <div class="modal" :class="{ active: isactive }">
      <a v-for="link in activedropdown" :href="link[1]" target="_blank">{{ link[0] }}</a>
    </div>
  </component>
</template>

<script>
/**
 * Used as main page navigation in templates.
 */
export default {
  name: "NavBar",
  status: "ready",
  release: "1.0.0",
  model: {
    prop: "active",
  },
  data() {
    return {
      isactive: false,
      activedropdown: "",
    }
  },
  props: {
    /**
     * The html element name used for the nav bar.
     */
    type: {
      type: String,
      default: "nav",
    },

    title: {
      type: String,
      default: "nav",
    },
    /**
     * State which tab is active when initiated (using name of the component).
     */
    active: {
      required: true,
      type: String,
    },
    /**
     * Menu items to be displayed on the nav bar.
     */
    navItems: {
      required: true,
      type: Array,
    },
  },
  computed: {
    localActive: {
      get() {
        return this.active
      },
      set(val) {
        this.$emit("input", val)
      },
    },
  },
}
</script>

<style lang="scss" scoped>
// Design Tokens with local scope
$color-nav-link: $color-bleu-de-france;
$color-nav-link-active: $color-bleu-de-france;

.modal {
  display: none;
}
.nav {
  @include reset;
  font-family: $font-text;
  font-size: $size-m;
  line-height: $line-height-m;
  color: $color-white;
  display: flex;
  width: 100%;
  display: flex;
  align-items: center;
  background: #0001de;
  transition: background 1s ease;
  position: fixed;
  top: 0;
  left: 0;
  padding: 0 20px;
  z-index: 999999;
  h2 {
    flex-grow: 1;
    margin-left: 0.75em;
  }
  ul.menu {
    padding: 0 !important;
    height: 100%;
    display: flex;

    align-items: center;
    li {
      list-style-type: none;
      margin: 0 !important;
      background: url("data:image/svg+xml,%3Csvg width='130' height='318' version='1' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='white' fill-rule='evenodd'%3E%3Cpath d='M0 284l35-19 19 34-35 19zM16 231l34-19 19 34-34 19zM31 178l34-19 19 34-34 19zM46 125l34-19 19 34-34 19zM61 72l34-19 19 34-34 19zM76 19l35-19 19 34-35 19z'/%3E%3C/g%3E%3C/svg%3E")
        no-repeat 100% 50%;
      background-size: 8px;
      padding: 10px 20px 10px 10px;
      &:last-child {
        background: none;
        padding: 0 10px;
      }
      a {
        color: white;
        font-weight: bold;
        margin: 0;
        text-decoration: none;
        position: relative;
      }
    }
  }
}

.dropdown {
  float: left;
  position: relative;
  z-index: 999999;
}

.dropdown .dropbtn {
  position: relative;
  z-index: 999999;
  font-size: 1em;
  font-weight: bold;
  border: none;
  outline: none;
  color: white;
  padding: 8px 3px;
  border-radius: 10px 10px 0 0;
  background-color: inherit;
  font-family: inherit;
  margin: 0;
  &:hover {
    cursor: pointer;
  }
}

.dropdown-content {
  display: none;
  position: absolute;
  background-color: #0000ca;
  min-width: 260px;
  box-shadow: 0px 3px 0px 0px rgba(0, 0, 0, 0.2);
  z-index: 1;
}

.dropdown-content a {
  float: none;
  color: black;
  padding: 12px 16px;
  text-decoration: none;
  display: block;
  border-bottom: 1px solid blue;
  text-align: left;
  &:last-child {
    padding: 12px 16px !important;
  }
}

.dropdown-content a:hover {
  background: blue;
}

.dropdown:hover .dropdown-content {
  display: block;
}

.dropdown-content.active {
  display: block !important;
}

.fixed {
  background: blue;
}

@media only screen and (max-width: 860px) {
  h2 {
    display: none;
  }

  .dropdown-content.active,
  .dropdown-content {
    display: none !important;
  }

  @keyframes modal {
    0% {
      opacity: 0;
    }
    100% {
      opacity: 1;
    }
  }

  .modal {
    width: 50%;
    background: white;
    position: fixed;
    top: 90px;
    right: 5%;
    padding: 5%;
    display: none;
    a {
      width: 100%;
      display: block;
      padding: 10px;
      text-decoration: none;
    }
  }
  .modal.active {
    display: block;
    opacity: 1;
    animation: modal 0.5s ease !important;
  }

  .dropdown .dropbtn {
    font-size: 1.3em;
  }
  .nav {
    padding: 0px 30px;
  }
  .logo {
    margin: 0 10px 0 0;
  }
  ul.menu {
    width: 100%;
    overflow: auto;
    white-space: nowrap;
    li {
      a {
        font-size: 1.3em;
      }
    }
  }
}
</style>

<docs>
  ```jsx
  <NavBar active="Dashboard" :navItems="[
    {name: 'Dashboard', component: 'Dashboard', href: '/example/'},
    {name: 'Posts', component: 'Posts', href: '/example/'},
    {name: 'Users', component: 'Users', href: '/example/'},
    {name: 'Settings', component: 'Settings', href: '/example/'}
  ]"/>
  ```
</docs>
