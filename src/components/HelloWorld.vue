<template>
  <input type="text" v-model="name" />
</template>

<script>

// input 需要
// 存储值
// 事件 onblur\onfocus\onclick\ondblclick\onchange\onkeydown

class FormData {
  constructor (inputs) {
    this.inputs = inputs || []
  }
  add (input) {
    this.inputs.push(input)
  }
  request () {
    let inputs = this.inputs
    let len = inputs.length
    let errors = {}
    let err
    let input
    for (var i = 0; i < len; i++) {
      input = inputs[i]
      err = typeof input.validation === 'function' && input.validation()
      if (err) errors[input.name] = err
    }
    return new Promise((resolve, reject) => {
      if (JSON.stringify(errors) === '{}') {
        reject(errors)
      } else {
        resolve()
      }
    })
  }
}

class InputData {
  constructor (opt) {
    this.value = opt.value
    this.name = opt.name
    this.validation = opt.validation
  }
}

class UserNameData extends InputData {
  constructor () {
    super({ name: `username` })
  }
}
// class PasswordData extends InputData {
//   constructor() {
//     super({ name: `password` });
//   }
// }

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data: function () {
    return {
      name: `miser1`
    }
  },
  mounted: function () {
    var form = new FormData()
    form.add(new UserNameData())
    form.request().then(res => {
      console.log(res)
    }).catch(error => {
      console.log(error)
    })
  }
}
</script>
