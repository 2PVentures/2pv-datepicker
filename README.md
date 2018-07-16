# Datepicker

Datepicker component is based on [flatpickr](https://github.com/chmln/flatpickr) for Vue Bulma.
Forked from [vue-bulma-datepicker](https://github.com/vue-bulma/datepicker) in order to implement reinitialisation for locale changes on the fly.

## Installation

```sh
npm install 2pv-vue-bulma-datepicker --save
```

## Examples

### Single Input

```html
<template>
  <datepicker placeholder="European Format ('d-m-Y')" :config="{ dateFormat: 'd-m-Y', static: true }"></datepicker>
</template>

<script>
import Datepicker from '2pv-vue-bulma-datepicker'

export default {
  components: {
    Datepicker
  }
}
</script>
```

### Wrap Input

```html
<template>
  <datepicker :config="{ wrap: true }" readonly>
    <a class="button" data-toggle><i class="fa fa-calendar"></i></a>
    <a class="button" data-clear><i class="fa fa-close"></i></a>
  </datepicker>
</template>

<script>
import Datepicker from '2pv-vue-bulma-datepicker'

export default {
  components: {
    Datepicker
  }
}
</script>
```

## Badges

![license-state](https://img.shields.io/badge/license-MIT-blue.svg)
![status](https://img.shields.io/badge/status-stable-green.svg)
