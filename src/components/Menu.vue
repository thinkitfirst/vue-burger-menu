<template>
  <div>
    <div id="sideNav" class="bm-menu">
      <span v-if="!isSearchMenu" class="bm-cross-button cross-style" @click.stop="closeMenu" :class="{ hidden: !hasCrossIcon }">
          <span v-for="(x, index) in 2" :key="x" class="bm-cross" :style="{ position: 'absolute', width: '3px', height: '14px',transform: index === 1 ? 'rotate(45deg)' : 'rotate(-45deg)'}">
          </span>
      </span>
      <div v-else class="bm-search-button search-style" @click.stop="closeSearchMenu">
        <slot name="searchHeader"></slot>
      </div>
      <nav class="bm-item-list">
        <slot v-if="!isSearchMenu" name="menu"></slot>
        <slot v-if="isSearchMenu" name="search"></slot>
      </nav>
    </div>
    <div class="bm-burger-button" @click.stop="openMenu" :class="{ hidden: !burgerIcon }">
        <span class="bm-burger-bars line-style" :style="{top:20 * (index * 2) + '%'}" v-for="(x, index) in 3" :key="index"></span>
    </div>
    <div class="bm-search-icon" @click.stop="openSearchMenu" :class="{ hidden: !hasSearchIcon }">
      <component v-bind:is="searchIcon"
        class="icon">
      </component>
    </div>
  </div>
</template>

<script>
    export default {
      name: 'menubar',
      data() {
        return {
          isSideBarOpen: false,
          isSearchMenu: false
        };
      },
      props: {
        isOpen: {
          type: Boolean,
          required: false
        },
        searchIcon: {
          type: [String],
          required: false,
          default: ''
        },
        right: {
          type: Boolean,
          required: false
        },
        width: {
          type: [String],
          required: false,
          default: '300'
        },
        disableEsc: {
          type: Boolean,
          required: false
        },
        noOverlay: {
          type: Boolean,
          required: false
        },
        onStateChange: {
          type: Function,
          required: false
        },
        burgerIcon: {
          type: Boolean,
          required: false,
          default: true
        },
        hasCrossIcon: {
          type: Boolean,
          required: false,
          default: true
        }
      },
      computed: {
        hasSearchIcon() {
          return this.searchIcon !== '';
        }
      },
      methods: {
        openMenu() {
          this.$emit('openMenu');
          this.isSearchMenu = false;
          this.isSideBarOpen = true;

          if (!this.noOverlay) {
            document.body.classList.add('bm-overlay');
          }
          if (this.right) {
            document.querySelector('.bm-menu').style.left = 'auto';
            document.querySelector('.bm-menu').style.right = '0px';
          }
          this.$nextTick(function() {
            document.getElementById('sideNav').style.width = this.width
              ? this.width
              : '300px';
          });
        },
        closeMenu() {
          this.$emit('closeMenu');
          this.isSideBarOpen = false;
          document.body.classList.remove('bm-overlay');
          document.getElementById('sideNav').style.width = '0px';
          if (this.isSearchMenu) {
            this.$emit('closeSearchMenu');
          }
        },
        openSearchMenu() {
          this.$emit('openSearchMenu');
          this.isSearchMenu = true;
          this.isSideBarOpen = true;

          if (!this.noOverlay) {
            document.body.classList.add('bm-overlay');
          }
          if (this.right) {
            document.querySelector('.bm-menu').style.left = 'auto';
            document.querySelector('.bm-menu').style.right = '0px';
          }
          this.$nextTick(function() {
            document.getElementById('sideNav').style.width = this.width
              ? this.width
              : '300px';
          });
        },
        closeSearchMenu() {
          this.$emit('closeSearchMenu');
          this.isSideBarOpen = false;
          document.body.classList.remove('bm-overlay');
          document.getElementById('sideNav').style.width = '0px';
        },
        closeMenuOnEsc(e) {
          e = e || window.event;
          if (e.key === 'Escape' || e.keyCode === 27) {
            document.getElementById('sideNav').style.width = '0px';
            document.body.style.backgroundColor = 'inherit';
            this.isSideBarOpen = false;
          }
        },
        documentClick(e) {
          let target = e.target;
          let eleName = target.nodeName;
          let triggers = ['a', 'option'];
          if (
          triggers.indexOf(eleName.toLowerCase()) > -1 ||
          (target.classList.contains('item') && 
          target.parentNode.classList.contains('menu')) ||
          (!this.closest(target, '.bm-item-list') && 
          !this.closest(target, '.bm-item-list')) && 
          (target.className !== 'bm-menu' && 
          this.isSideBarOpen)
          ) {
            this.closeMenu();
          }
        },
        closest(el, selector) {
          let matchesFn;
          ['matches','webkitMatchesSelector','mozMatchesSelector','msMatchesSelector','oMatchesSelector'].some(function(fn) {
              if (typeof document.body[fn] == 'function') {
                  matchesFn = fn;
                  return true;
              }
              return false;
          });

          let parent;

          while (el) {
            parent = el.parentElement;
            if (parent && parent[matchesFn](selector)) {
                return parent;
            }
            el = parent;
          }

          return null;
        }
      },
      mounted() {
        if (!this.disableEsc) {
          document.addEventListener('keyup', this.closeMenuOnEsc);
        }
      },
      created: function() {
        document.addEventListener('click', this.documentClick);
        document.addEventListener('click', this.closeSearchMenu);
      },
      destroyed: function() {
        document.removeEventListener('keyup', this.closeMenuOnEsc);
        document.removeEventListener('click', this.documentClick);
        document.removeEventListener('click', this.closeSearchMenu);
      },
      watch: {
        isOpen: {
          deep: true,
          immediate: true,
          handler(newValue, oldValue) {
            if (!oldValue && newValue) {
              this.openMenu()
            }
            if (oldValue && !newValue) {
              this.closeMenu()
            }
          }
        },
        right: {
          deep: true,
          immediate: true,
          handler(oldValue, newValue) {
            if (oldValue) {
              this.$nextTick(() => {
                document.querySelector('.bm-burger-button').style.left = 'auto';
                document.querySelector('.bm-burger-button').style.right = '36px';
                document.querySelector('.bm-menu').style.left = 'auto';
                document.querySelector('.bm-menu').style.right = '0px';
              });
            }
            if (newValue) {
              if (
                document.querySelector('.bm-burger-button').hasAttribute('style')
              ) {
                document
                  .querySelector('.bm-burger-button')
                  .removeAttribute('style');
                document.getElementById('sideNav').style.right = 'auto';
              }
            }
          }
        }
      }
    };
</script>

<style>
    html {
      height: 100%;
    }
    .bm-burger-button {
      position: absolute;
      width: 36px;
      height: 30px;
      left: 36px;
      top: 36px;
      cursor: pointer;
    }
    .bm-burger-button.hidden {
      display: none;
    }
    .bm-burger-bars {
      background-color: #373a47;
    }
    .line-style {
      position: absolute;
      height: 20%;
      left: 0;
      right: 0;
    }
    .cross-style {
      position: absolute;
      top: 12px;
      right: 2px;
      cursor: pointer;
    }
    .bm-cross {
      background: #bdc3c7;
    }
    .bm-cross-button {
      height: 24px;
      width: 24px;
    }
    .bm-cross-button.hidden {
      display: none;
    }
    .bm-menu {
      height: 100%; /* 100% Full-height */
      width: 0; /* 0 width - change this with JavaScript */
      position: fixed; /* Stay in place */
      z-index: 1000; /* Stay on top */
      top: 0;
      left: 0;
      background-color: rgb(63, 63, 65); /* Black*/
      overflow-x: hidden; /* Disable horizontal scroll */
      padding-top: 60px; /* Place content 60px from the top */
      transition: 0.5s; /*0.5 second transition effect to slide in the sidenav*/
    }

    .bm-overlay {
      background: rgba(0, 0, 0, 0.3);
    }
    .bm-item-list {
      color: #b8b7ad;
      margin-left: 10%;
      font-size: 20px;
    }
    .bm-item-list > * {
      display: flex;
      text-decoration: none;
      padding: 0.7em;
    }
    .bm-item-list > * > span {
      margin-left: 10px;
      font-weight: 700;
      color: white;
    }
</style>

