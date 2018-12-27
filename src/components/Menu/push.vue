<template>
    <div>
        <Menu v-bind="this.$attrs" @openMenu="push" @openSearchMenu="searchPush" @closeMenu="pull" @closeSearchMenu="searchPull" ref="theMenu">
            <template slot="menuHeader">
              <slot name="menuHead"></slot>
            </template>
            <template slot="searchHeader">
              <slot name="searchHead"></slot>
            </template>
            <template slot="menu">
              <slot name="menuNav"></slot>
            </template>
            <template slot="search">
              <slot name="searchNav"></slot>
            </template>
        </Menu>
    </div>
</template>

<script>
    import Menu from '../Menu';
    export default {
      name: 'push',
      components: {
        Menu: Menu
      },
      props: {
        closeSearchMenu: {
          type: Boolean,
          required: false,
          default: false
        },
        openSearchMenu: {
          type: Boolean,
          required: false,
          default: false
        }
      },
      watch: {
        closeSearchMenu(newValue, oldValue) {
          if (!oldValue && newValue) {
            this.$refs.theMenu.closeSearchMenu()
          }
        },
        openSearchMenu(newValue, oldValue) {
          if (!oldValue && newValue) {
            this.$refs.theMenu.openSearchMenu()
          }
        }
      },
      methods: {
        openMenu () {
            this.$emit("openMenu")
        },
        closeMenu () {
            this.$emit("closeMenu")
        },
        openSearch () {
          this.$emit('openSearchMenu')
        },
        closeSearch () {
          this.$emit('closeSearchMenu')
        },
        push() {
          this.openMenu()
          let width = this.$attrs.width ? this.$attrs.width : '300px';

          document.body.style.overflowX = 'hidden';

          if (this.$attrs.right) {
            document.querySelector(
              '#page-wrap'
            ).style.transform = `translate3d(-${width}, 0px, 0px )`;
          } else {
            document.querySelector(
              '#page-wrap'
            ).style.transform = `translate3d(${width}, 0px, 0px )`;
          }

          document.querySelector('#page-wrap').style.transition =
            'all 0.5s ease 0s';
        },
        pull() {
          this.closeMenu()
          document.querySelector('#page-wrap').style.transition =
            'all 0.5s ease 0s';
          document.querySelector('#page-wrap').style.transform = '';
          document.body.removeAttribute('style');
        },
        searchPush() {
          this.openSearch()
          let width = this.$attrs.width ? this.$attrs.width : '300px';

          document.body.style.overflowX = 'hidden';

          if (this.$attrs.right) {
            document.querySelector(
              '#page-wrap'
            ).style.transform = `translate3d(-${width}, 0px, 0px )`;
          } else {
            document.querySelector(
              '#page-wrap'
            ).style.transform = `translate3d(${width}, 0px, 0px )`;
          }

          document.querySelector('#page-wrap').style.transition =
            'all 0.5s ease 0s';
        },
        searchPull() {
          this.closeSearch()
          document.querySelector('#page-wrap').style.transition =
            'all 0.5s ease 0s';
          document.querySelector('#page-wrap').style.transform = '';
          document.body.removeAttribute('style');
        }
      }
    };
</script>


