<template>
<aside :id="id"
    class="mdc-dialog"
    role="alertdialog"
    :aria-labelledby="labelId"
    :aria-describedby="descriptionId">
  <div class="mdc-dialog__surface" :class="surfaceClasses">
    <header class="mdc-dialog__header">
      <slot name="header">
        <h2 :id="labelId"
            class="mdc-dialog__header__title mdc-theme--text-primary-on-light">
          {{ title }}
        </h2>
      </slot>
    </header>
    <section :id="descriptionId" :class="bodyClasses"
        class="mdc-dialog__body">
      <slot></slot>
    </section>
    <footer class="mdc-dialog__footer">
      <slot name="footer">
        <v-button
            class="mdc-dialog__footer__button mdc-dialog__footer__button--cancel"
            :primary="true"
            v-if="cancelText">
          {{ cancelText }}
        </v-button>
        <v-button
            class="mdc-dialog__footer__button mdc-dialog__footer__button--accept"
            :primary="true"
            v-if="acceptText">
          {{ acceptText }}
        </v-button>
      </slot>
    </footer>
  </div>
  <div class="mdc-dialog__backdrop"></div>
</aside>
</template>

<script>
import {MDCDialog} from '@material/dialog';

import Button from './Button';

export default {
  name: 'v-dialog',
  components: {[Button.name]: Button},

  props: {
    id: {
      type: String,
      required: true
    },
    scrollable: {
      type: Boolean,
      default: false
    },
    title: {
      type: String,
      default: ''
    },
    acceptText: {
      type: String,
      default: ''
    },
    cancelText: {
      type: String,
      default: ''
    },
    showDialog: {
      type: Boolean,
      default: false
    }
  },

  computed: {
    bodyClasses: function() {
      return {
        'mdc-dialog__body--scrollable': this.scrollable
      };
    },
    surfaceClasses: function() {
      return {
        scrollable: this.scrollable
      };
    },
    labelId: function() {
      return this.id ? `${this.id}__label` : false;
    },
    descriptionId: function() {
      return this.id ? `${this.id}__description` : false;
    }
  },

  methods: {
    onAccept: function() {
      this.$emit('accept');
    },

    onCancel: function() {
      this.$emit('cancel');
    }
  },

  mounted: function() {
    this.dialog = new MDCDialog(document.querySelector(`#${this.id}`));
    this.dialog.listen('MDCDialog:accept', this.onAccept);
    this.dialog.listen('MDCDialog:cancel', this.onCancel);
  },

  watch: {
    showDialog: function(show) {
      if (show) {
        this.dialog.show();
      } else {
        this.dialog.close();
      }
    }
  }
};
</script>

<style lang="scss">
$mdc-theme-primary: #1abc9c;

@import '@material/dialog/mdc-dialog';
@import '@material/theme/mdc-theme';

/* tablets */
@media (min-width: 768px) {
  .mdc-dialog__surface {
    width: auto;
    min-width: 456px;
    max-width: 660px;
  }

  .scrollable {
    min-width: calc(456px + 30px);
    max-width: calc(660px + 30px);
  }
}
</style>
