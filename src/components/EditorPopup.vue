<template>
  <div
    :class="{ 'has-close': isCloseable }"
    class="EditorPopup"
    @mouseenter="onMouseEnter"
    @mouseleave="onMouseLeave"
  >
    <div class="EditorPopup-content">
      <template v-for="(section, index) in content">
        <span
          v-if="section.content"
          :key="'content-' + index"
          :class="getSectionClass(section.type)"
          class="EditorPopup-contentSection"
        >{{ section.content }}</span>
        <span
          v-if="section.partials"
          :key="'partials-' + index"
          class="EditorPopup-partial"
        >
          <span
            v-for="(partial, partialIndex) of section.partials"
            :key="partialIndex"
            :class="getSectionClass(partial.type)"
            class="EditorPopup-contentSection is-partial"
          >{{ partial.content }}</span>
        </span>
        <span
          v-if="index != content.length - 1"
          :key="'separator-' + index"
          class="EditorPopup-separator"
        >
        </span>
      </template>
    </div>
    <button
      v-show="isCloseable"
      class="EditorPopup-close"
      @click="onPopupClose"
    >
      <span>&times;</span>
    </button>
  </div>
</template>

<script>

import cuid from 'cuid';

import { PopupType } from '@/constants/popups';

export default {
  props: {
    isCloseable: {
      type: Boolean,
      default: () => false,
    },
    cm: {
      type: Object,
      required: true,
    },
  },

  data() {
    return {
      content: [],
      isShowing: false,
      isDestroyed: false,
    };
  },

  created() {
    this.tokens = [];
  },

  beforeDestroy() {
    this.cancelAllHide();
    this.cancelAllShow();
    this.cm = null;
  },

  methods: {
    onMouseEnter(e) {
      this.$emit('popup-mouse-enter', e);
    },

    onMouseLeave(e) {
      this.$emit('popup-mouse-leave', e);
    },

    onPopupClose() {
      this.hide();
    },

    getSectionClass(type) {
      return {
        'is-info': type === PopupType.Info,
        'is-code': type === PopupType.Code,
      };
    },

    /**
     * @example
     *   popup.show({
     *    content: [{
     *        content: `This point can not be highlighted because it has been modified`,
     *        type: 'info',
     *      }, {
     *        partials: [{
     *          content: `${loc.start.line}:${loc.start.column}`,
     *        }, {
     *          type: 'code',
     *          content: this.beautify(this.coverage.values[index]),
     *        }],
     *      },
     *    ],
     *    loc: {
     *      line,
     *      column: 0,
     *    },
     *  });
     */

    // TODO: scrollIntoView=true,
    show({
      content=[],
      delay,
      loc,
    }={}) {
      const token = {
        type: 'show',
      };

      token.promise =
        new Promise((resolve) => {
          token.id = cuid();
          token.timeout = setTimeout(() => {
            this.cm.addWidget({
              line: loc.line,
              ch: loc.column,
            }, this.$el);

            this.content = content;

            this.cm.scrollIntoView({
              line: loc.line,
              ch: loc.column + 50,
            }, 200);

            this.removeToken([token]);

            this.isShowing = true;

            resolve();
          }, delay);
        });

      this.addToken([token]);

      return token;
    },

    hide({ delay=0 }={}) {
      const token = {
        type: 'hide',
      };

      token.promise =
        new Promise((resolve) => {
          token.id = cuid();
          token.timeout = setTimeout(() => {
            if (this.$el.parentNode) {
              this.$el.parentNode.removeChild(this.$el);
            }

            this.removeToken([token]);
            this.isShowing = false;

            resolve();
          }, delay);
        });

      this.addToken([token]);

      return token;
    },

    addToken(tokens) {
      this.tokens = this.tokens.concat(tokens);
    },

    removeToken(tokens) {
      this.tokens =
        this.tokens.filter((token) => tokens.some(t => t.id === token.id));
    },

    cancelHide(tokens) {
      tokens.forEach(token => clearTimeout(token.timeout));

      this.removeToken(tokens);
    },

    cancelShow(tokens) {
      tokens.forEach(token => clearTimeout(token.timeout));

      this.removeToken(tokens);
    },

    cancelAllHide() {
      this.cancelHide(this.tokens.filter(token => token.type === 'hide'));
    },

    cancelAllShow() {
      this.cancelShow(this.tokens.filter(token => token.type === 'show'));
    },

    destroy() {
      return this.hide().then(() => {
        this.cm = null;
      });
    },
  },
};

</script>

<style lang="scss">

.EditorPopup {
  background-color: $app-editor-popup-background;
  border-radius: 4px;
  box-shadow: $app-box-shadow-1;
  display: inline-block;
  justify-content: space-between;
  max-height: 233px;
  max-width: 420px;
  overflow: auto;
  transition: 400ms opacity;
  z-index: $app-walthrough-popup-layer;
}

.EditorPopup-contentSection {
  display: block;
  line-height: 1.6;
  max-width: 100%;
  padding: $app-editor-popup-padding-y $app-editor-popup-padding-r $app-editor-popup-padding-y $app-editor-popup-padding-l;

  &.is-info {
    background-color: rgba(black, 0.14);
    border: 1px solid rgba(black, 0.06);
    border-radius: 4px;
    margin: 8px 6px;
    white-space: normal;
  }

  &.is-code {
    overflow: auto;
    white-space: pre;
  }

  &.is-partial:not(:last-child) {
    border-right: 1px solid rgba(black, 0.08);
    padding-right: $app-editor-popup-padding-r;
  }
}

.EditorPopup-partial {
  display: flex;
}

.EditorPopup-separator {
  border-bottom: 1px solid rgba(black, 0.16);
  display: block;
  width: 100%;

  &:before {
    content: '';
    display: block;
    width: 100%;
  }
}

.EditorPopup-close {
  display: none;
  background-color: $color-error;
  border: 0;
  border-radius: 0 4px 4px 0;
  color: white;
  cursor: pointer;
  font-size: 14px;
  line-height: 1;
  line-height: 24px;
  outline: 0;
  text-align: center;
  user-select: none;
  width: 22px;

  > span {
    line-height: inherit;
  }
}

</style>
