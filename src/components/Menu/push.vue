<template>
    <div>
        <Menu v-bind="this.$attrs" @openMenu="push" @openSearchMenu="searchPush" @closeMenu="pull" @closeSearchMenu="searchPull">
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
      methods: {
        openMenu () {
            this.$emit("openMenu")
        },
        closeMenu () {
            this.$emit("closeMenu")
        },
        openSearchMenu () {
          this.$emit('openSearchMenu')
        },
        closeSearchMenu () {
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
          this.openSearchMenu()
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
          this.closeSearchMenu()
          document.querySelector('#page-wrap').style.transition =
            'all 0.5s ease 0s';
          document.querySelector('#page-wrap').style.transform = '';
          document.body.removeAttribute('style');
        }
      }
    };
</script>


