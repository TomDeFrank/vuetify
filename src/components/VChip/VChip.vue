<script>
  import Toggleable from '../../mixins/toggleable'

  export default {
    name: 'v-chip',

    mixins: [Toggleable],

    props: {
      close: Boolean,
      label: Boolean,
      outline: Boolean,
      small: Boolean,
      value: {
        type: Boolean,
        default: true
      }
    },

    computed: {
      classes () {
        return {
          'chip': true,
          'chip--label': this.label,
          'chip--outline': this.outline,
          'chip--small': this.small,
          'chip--removable': this.close
        }
      }
    },

    render (h) {
      const children = [this.$slots.default]
      const data = {
        'class': this.classes,
        attrs: { tabindex: -1 },
        directives: [{
          name: 'show',
          value: this.isActive
        }],
        on: this.$listeners
      }

      if (this.close) {
        children.push(h('a', {
          'class': 'chip__close',
          domProps: { href: 'javascript:;' },
          on: {
            click: e => {
              e.stopPropagation()

              this.$emit('input', false)
            }
          }
        }, [
          h('v-icon', { props: { right: true } }, 'cancel')
        ]))
      }

      return h('span', data, children)
    }
  }
</script>

<style lang="stylus" src="../../stylus/components/_chips.styl"></style>
