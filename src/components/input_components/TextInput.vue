
<template>	
		<div class="display-inline" v-if="showInputField">
			<label 
				:for="currentKey">
				{{ schema.title }}
			</label>
			<input 
				id="text-input" 
				type="text" 
				:name="currentKey" 
				v-model="value[currentKey]">
		</div>	
</template>

<script>
export default {
  name: 'TextInput',
  props: {
    schema: {
      type: Object,
      default() {
        return {}
      }
    },
    value: {
      type: Object,
      default() {
        return {}
      }
    },
    currentKey: {
      type: String,
      default() {
        return ""
      }
    }
  },
  data() {
    return {
      placeholder: this.getPlaceholder()     
    }
  },
  methods: {
    clearInput() {
      // this.value[this.schema.attrs.fieldName] = null
      let initValue = null
      switch (this.$options.name) {
        case 'TextInput':
          initValue = ''
          this.$set(this.value, this.currentKey, initValue)
          break
        case 'NumberInput':
          initValue = ''
          this.$set(this.value, this.currentKey, initValue)
          break
        case 'Checkbox':
          initValue = false
          this.$set(this.value, this.currentKey, initValue)
          break
        case 'CheckList':
          initValue = []
          this.$set(this.value, this.currentKey, initValue)
          break
        case 'RadioInput':
          initValue = ''
          this.$set(this.value, this.currentKey, initValue)
          break
        case 'SelectDate':
          initValue = ''
          this.$set(this.value, this.currentKey, initValue)
          break
        case 'SelectList':
          initValue = ''
          this.$set(this.value, this.currentKey, initValue)
          break
      }
    },
    getPlaceholder() {
      if (typeof this.schema.attrs !== 'undefined') {
        if (typeof this.schema.attrs.placeholder !== 'undefined') {
          return this.schema.attrs.placeholder
        }
      }
      return ''
    }
  },
  computed: {
    showInputField() {
      let schemaAttrs = this.schema.attrs

      // dependsOn name is Array?
      if (schemaAttrs) {
        if (schemaAttrs.dependsOn) {
          if (schemaAttrs.dependsOn.values && schemaAttrs.dependsOn.name) {
            // if(typeof schemaAttrs.dependsOn.values !== 'undefined' && typeof schemaAttrs.dependsOn.name !== 'undefined'){
            if (Array.isArray(this.value[schemaAttrs.dependsOn.name])) {
              for (let i = 0; i < schemaAttrs.dependsOn.values.length; i++) {
                if (
                  this.value[schemaAttrs.dependsOn.name].indexOf(
                    schemaAttrs.dependsOn.values[i]
                  ) !== -1
                ) {
                  return true
                } else {
                  this.clearInput()
                  return false
                }
              }
            } else {
              if (
                schemaAttrs.dependsOn.values.indexOf(
                  this.value[schemaAttrs.dependsOn.name]
                ) !== -1
              ) {
                return true
              } else {
                this.clearInput()
                return false
              }
            }
          }
          return true
        }
        return true
      }
      return true
    }
  }
}
</script>

<style>
</style>


