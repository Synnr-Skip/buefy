<template>
    <div class="radio-group" :class="{ 'field has-addons': isRadioButtonGroup }">
        <slot></slot>
    </div>
</template>

<script>
    export default {
        name: 'bRadioGroup',
        props: {
            value: [String, Number, Boolean],
            buttonSize: String
        },
        data() {
            return {
                isRadioGroupComponent: true, // Used internally by Radio and RadioButton
                isRadioButtonGroup: false // Used internally by RadioButton
            }
        },
        watch: {
            /**
             * Set checked state on the correct children Radio
             * or RadioButton when v-model change.
             */
            value() {
                this.initChecked()
            }
        },
        methods: {
            /**
             * This is called from a child Radio or RadioButton.
             * Emit input event to update the user v-model.
             */
            updateValue(value, event) {
                this.$emit('change', value, event)
                this.$emit('input', value)
            },

            /**
             * Set checked state on the correct children Radio or RadioButton based on v-model.
             */
            initChecked() {
                this.$children.forEach((child) => {
                    child.size = this.buttonSize
                    this.isRadioButtonGroup = child.isRadioButtonComponent
                    child.isChecked = this.value === child.value
                })
            }
        },
        mounted() {
            this.initChecked()
        }
    }
</script>
