<template>
  <div class="gloomy-tag--container" @click="cursorToInput">
    <span
      v-for="item in focusArray"
      :key="item.tag"
      :class="['gloomy-tag--tag', classNames?.tag, item.focus ? 'gloomy-tag--focus' : '']"
    >
      <span>{{ item.tag }}</span>
      <button
        v-if="!disabled"
        type="button"
        @click.stop="handleTagRemove(item.tag)"
        aria-label="'remove ' + item.tag"
      >
        &#10005;
      </button>
    </span>
    <input
      class="gloomy-tag--input"
      :class="classNames?.input"
      type="text"
      :name="name"
      :placeholder="placeHolder"
      @keydown="handleKeyDown"
      @blur="onBlur"
      :disabled="disabled"
      @keyup="onKeyUp"
      ref="inputRef"
    />
  </div>
</template>

<script>
export default {
  name: 'GloomyTags',
  props: {
    name: String,
    placeHolder: String,
    state: Array,
    setState: Function,
    onChange: Function,
    onBlur: Function,
    separators: {
      type: Array,
      default: () => []
    },
    disableBackspaceRemove: {
      type: Boolean,
      default: false
    },
    onExisting: Function,
    onRemoved: Function,
    disabled: {
      type: Boolean,
      default: false
    },
    isEditOnRemove: {
      type: Boolean,
      default: false
    },
    beforeAddValidate: Function,
    onKeyUp: Function,
    classNames: Object,
    throttleTime: {
      type: Number,
      default: 300
    }
  },
  data() {
    return {
      focusArray: [],
      throttle: null
    }
  },
  computed: {
    isFocus() {
      return this.focusArray.some((e) => e.focus)
    }
  },
  methods: {
    cursorToInput(e) {
      if (!this.$refs.inputRef || e.target.className !== 'gloomy-tag--container') return
      this.$refs.inputRef.focus()
    },
    handleKeyDown(event) {
      const inputValue = event.currentTarget.value.replace(/\s/gi, '')
      const key = event.key

      if (inputValue && (this.separators.includes(key) || ['Enter', ' ', ','].includes(key))) {
        event.preventDefault()

        if (this.beforeAddValidate && !this.beforeAddValidate(inputValue, this.state)) {
          return
        }

        if (!this.throttle) {
          this.throttle = setTimeout(() => {
            this.throttle = null
          }, this.throttleTime)
        } else {
          console.warn('react-tag-maker throttling!')
          return
        }

        if (!this.state.includes(inputValue)) {
          this.setState([...this.state, inputValue])
          event.currentTarget.value = ''
        } else {
          this.onExisting && this.onExisting(inputValue)
        }
      }

      if (!inputValue && !this.disableBackspaceRemove && this.state.length && key === 'Backspace') {
        event.preventDefault()
        if (this.isEditOnRemove) {
          event.currentTarget.value = `${this.state.slice(-1)} `
        } else if (this.isFocus) {
          this.setState([...this.state.slice(0, -1)])
        } else if (!this.isFocus && this.state.length > 0) {
          const tempFocusArray = [...this.focusArray]
          if (tempFocusArray[tempFocusArray.length - 1]) {
            tempFocusArray[tempFocusArray.length - 1].focus = true
            this.focusArray = tempFocusArray
          }
        }
      } else {
        const tempFocusArray = [...this.focusArray]
        if (tempFocusArray[tempFocusArray.length - 1]) {
          tempFocusArray[tempFocusArray.length - 1].focus = false
          this.focusArray = tempFocusArray
        }
      }
    },
    handleTagRemove(tag) {
      this.setState(this.state.filter((t) => t !== tag))
      this.onRemoved && this.onRemoved(tag)
    }
  },
  watch: {
    state(newState) {
      const tempFocusArray = Array.isArray(newState)
        ? newState.map((e) => ({ tag: e, focus: false }))
        : []
      this.focusArray = tempFocusArray
      this.onChange && this.onChange(newState)
    }
  }
}
</script>
<style scoped>
/* Vue scoped style */
.gloomy-tag--container {
  box-sizing: border-box;
  transition: all 0.2s ease;
  align-items: center;
  background: #fff;
  border: 1px solid #ccc;
  border-radius: 5px;
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  line-height: 1.4;
  padding: 8px;
  width: 375px;
  cursor: text;

  @media (max-width: 600px) {
    width: 100%;
  }

  &:focus-within {
    border-color: #1d77fe;
    box-shadow: #1d77fe 0 0 0 1px;
  }

  &,
  * {
    box-sizing: border-box;
    transition: all 0.2s ease;
  }
  .gloomy-tag--input {
    border: 0;
    outline: 0;
    font-size: inherit;
    line-height: inherit;
    width: 50%;
    background: #fff;
    color: #000;
  }

  .gloomy-tag--tag {
    align-items: center;
    background: #dbe3eb;
    border: 1px solid #b6b5b5;
    border-radius: 10px;
    display: inline-flex;
    justify-content: center;
    padding: 2.5px 4px;
    opacity: 1;
    color: #000;
    &:has(button:hover) {
      background: #ffe2e2;
    }
    &.gloomy-tag--focus {
      background: #ffe2e2;
    }
    button {
      background: none;
      border: 0;
      border-radius: 50%;
      cursor: pointer;
      line-height: inherit;
      padding: 0 8px;
      font-weight: 900;
      color: #000;
      &:hover {
        color: #e53e3e;
      }
    }
    &.disappearing {
      transition: 0.15s;
      opacity: 0;
      background: #ffaaaa;
    }
  }
}
</style>
