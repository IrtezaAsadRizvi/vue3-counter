# Vue 3 Counter 🔢


![[banner]](https://i.ibb.co/fks3WVp/vue3-counter-banner.gif)


A highly customised numeric counter component for Vue 3 Applications.



## ✨ Features

- ✨ Customizable
- ✨ Multiple icons and style alignments
- ✨ Auto adjusted input width
- ✨ Support Typescript



## 🎯 install

```bash
npm install vue3-counter --save
```

## 🚀 Usage

Import and Use 
```html
<script setup lang="ts">
import Counter from 'vue3-counter'
const config = {
    inputColor: '#3A78EF',
    countUpColor: '#3A78EF',
    countDownColor: '#3A78EF',
    initial: 5,
    arrow: true,
    vertical: false,
    max: 10,
    min: 0
}
</script>

<template>
    <Counter :config="config" @change="handleChange($event)" />
</template>
```

## Customization
### `config` prop properties

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| `inputColor` | `string` | `"#212121"` | Background color of the input field. |
| `textColor` | `string` | `"#FFFFFF"` | Color of the input text. |
| `countUpColor` | `string` | `"#212121"` | Color of the count up button. |
| `countDownColor`&nbsp;&nbsp; | `string` | `"#212121"` | Color of the count down button. |
| `initial` | `number` | `0` | Initial value of the counter. |
| `min` | `number` | `undefined` | Minimum value of the counter. |
| `max` | `number` | `undefined` | Maximum value of the counter. |
| `arrow` | `boolean` | `false` | If true arrows would show intead of +,- |
| `vertical` | `boolean` | `false` | Vertical or horizontal |

### events
| Name | Fires when |
| --- | --- |
| `countUp` | Count up button is clicked. |
| `countDown`&nbsp;&nbsp;&nbsp; | Count down button is clicked. |
| `input` | User types on the input. |
| `change` | Everytime the value changes. |


## License

[MIT]