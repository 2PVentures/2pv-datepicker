<template>
  <component :value="date" :placeholder="placeholder" 
    :inputClass="inputClass" :alignment="alignment" 
    :is="wrap ? 'WrapperInput' : 'SingleInput'">
    <slot></slot>
  </component>
</template>

<script>
import BasicInput from './BasicInput'
import SingleInput from './SingleInput'
import WrapperInput from './WrapperInput'
import Flatpickr from '../node_modules/flatpickr'

import ru from '../node_modules/flatpickr/src/l10n/ru';
import de from '../node_modules/flatpickr/src/l10n/de';
import fr from '../node_modules/flatpickr/src/l10n/fr';
import ja from '../node_modules/flatpickr/src/l10n/ja';

const I18n = {
  'default': 'default',
  'ru': ru.ru,
  'de': de.de,
  'fr': fr.fr,
  'ja': ja.ja
}

export default {
  mixins: [BasicInput],

  props: {
    alignment: String,
    config: {
      type: Object,
      default: () => ({})
    }
  },

  components: {
    SingleInput,
    WrapperInput
  },

  data () {
    return {
      datepicker: null,
      selectedDates: null
    }
  },

  mounted () {
    if (!this.datepicker) {
      this.config.onValueUpdate = this.dateUpdated
      this.datepicker = new Flatpickr(this.$el, this.config)
      this.setDate(this.value)
    }

    this.$watch('config', (newConfig) => {
      //setup the constructor to be localized to the new config locale
      if (newConfig.locale){
        Flatpickr.localize(I18n[newConfig.locale]);
      }
      //"redraw" the datepicker widget
      this.redraw(newConfig);
    });

    this.$watch('value', this.setDate)
  },

  beforeDestroy () {
    if (this.datepicker && !this.static) {
      this.datepicker.destroy()
      this.datepicker = null
    }
  },

  methods: {
    redraw (newConfig) {
      //instead of updating the config, we regenerate the flatpickr altogether
      //this allows the new locale settings to take effect
      this.datepicker = new Flatpickr(this.$el, this.config);
      this.datepicker.redraw();
      this.datepicker.jumpToDate();
    },
    setDate (newDate, oldDate) {
      newDate && this.datepicker.setDate(newDate)
    },
    dateUpdated (selectedDates, dateStr) {
      this.date = dateStr
    }
  },

  computed: {
    wrap () {
      return !!this.config.wrap
    },
    static () {
      return !!this.config.static
    },
    name () {
      return this.wrap ? 'wrapperInput' : 'singleInput'
    },
    date: {
      get() {
        return this.selectedDates || this.value
      },
      set(newValue) {
        if (this.selectedDates !== newValue) {
          this.selectedDates = newValue
          this.$emit('input', newValue)
        }
      }
    }
  }
}
</script>

<style lang="stylus">
$calendar_background = #ffffff
$calendar_border_color = #d3d6db

$months_color = #111
$months_background = transparent

$weekdays_background = transparent

$day_text_color = #222324
$day_hover_background_color = #d3d6db

$today_color = #ed6c63
$selected_day_background = #1fc8db

@import '../node_modules/flatpickr/src/style/flatpickr.styl'

.flatpickr-calendar.hasWeeks
  width: auto !important
</style>
