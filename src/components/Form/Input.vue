<template>
    <div>
        <h3><label :for="inputData.name">{{ inputData.label }}<span v-if="inputData.required">&#9913;</span></label></h3>
        <div v-if="inputData.type === 'text' || inputData.type ==='Date' || inputData.type ==='number'|| inputData.type ==='checkbox'">
            <input
                    :type="inputData.type"
                    :id="inputData.name"
                    :class="v.formData[inputData.name] !== undefined ? v.formData[inputData.name].$error  ? 'is-invalid' : '' : ''"
                    v-model.trim="formData[inputData.name]"
            >
        </div>
        <div v-if="inputData.type === 'radio'" :class="inputData.name">
            <label>Мужчина</label>
            <input
                    :type="inputData.type"
                    :id="inputData.name"
                    value="male"
                    v-model="formData[inputData.name]">
            <label>Женщина</label>
            <input
                    :type="inputData.type"
                    :id="inputData.name"
                    value="female"
                    v-model="formData[inputData.name]" >
        </div>
        <div v-if="inputData.type === 'select'" >
            <select
                    :id="inputData.name"
                    :class="v.formData[inputData.name] ? v.formData[inputData.name].$error  ? 'is-invalid' : '' : ''"
                    v-model="formData[inputData.name]">
                <option v-if="inputData.name !== 'doctor'" value="" disabled selected>Выбирите {{inputData.label}}</option>
                <option v-if="inputData.name === 'doctor'" value="" disabled selected>Выбирите лечащего врача</option>
                <option
                        v-for="option in inputData.options"
                        :key="option"
                        v-bind:option="option"
                        :value="option"
                >{{option}}</option>
            </select>
        </div>
        <div v-if="inputData.type === 'm-select'">
            <select
                    :id="inputData.name"
                    :class="v.formData[inputData.name] !== undefined ? v.formData[inputData.name].$error  ? 'is-invalid' : '' : ''"
                    v-model="formData[inputData.name]"
                    :size="inputData.options.length"
                    multiple >
                <option
                        v-for="option in inputData.options"
                        :key="option"
                        v-bind:option="option"
                        :value="option"
                >{{option}}</option>
            </select>
        </div>
        <div v-if="v.formData[inputData.name]">
            <p v-if="v.formData[inputData.name].$dirty && !v.formData[inputData.name].required
                && v.formData[inputData.name].$error && inputData.name !== 'phoneNumber'" class="invalid-feedback">
                Обязательное поле!
            </p>
            <p v-if="v.formData.phoneNumber.$dirty && !v.formData.phoneNumber.length && inputData.name=== 'phoneNumber'"
                class="invalid-feedback">
                Количество цифр 11, первая 7 !!!
            </p>

        </div>
    </div>
</template>

<script>
export default {
    props: ['inputData', 'formData','v']
}
</script>

<style scoped lang="sass">
span
    position: relative
    bottom: 5px
input, select
    width: 100%
    font-size: 15px
    border: 1px solid #dde0e2
    background-color: #f8fbfd
    border-radius: 3px
    padding: 12px
    box-sizing: border-box
    display: block
    margin-top: -15px
    -moz-appearance: textfield
input::-webkit-inner-spin-button
    display: none
input:focus, select:focus
    outline: none
    border: 1px solid #29B0D9
select
    -webkit-appearance: none
    -moz-appearance: none
    appearance: none
    cursor: pointer
label
    font-weight: 700
    color: rgba(147,159,173,.84)
    font-size: .8rem
    text-transform: uppercase
    line-height: .9375rem
    letter-spacing: 1.2px
input[type="checkbox"]
    position: relative
    cursor: pointer
    width: 60px
    height: 20px
    -webkit-appearance: none
    background: #c6c6c6
    outline: none
    border-radius: 50px
    box-shadow: inset 0 0 5px rgba(0,0,0,.2)
    transition: .5s
input:checked[type="checkbox"]
    background: #6699ff
input[type="checkbox"]:before
    cursor: pointer
    content: ''
    position: absolute
    width: 25px
    height: 25px
    border-radius: 20px
    top: 0
    left: 0
    background: #fff
    transform: scale(1.1)
    box-shadow: 0 2px 5px rgba(0,0,0,.2)
    transition: .5s
input:checked[type="checkbox"]:before
    left: 40px
#gender
    width: 10%
div.gender
    display: flex
input[type="radio"]
    position: relative
    top: 13px
    right: 5px
    width: 1.2em
    height: 1.2em
    cursor: pointer
    background: #c6c6c6
.is-invalid
    border: 1px solid #f57f6c
.invalid-feedback
    position: relative
    bottom: 5px
    left: 10px
    color: #f57f6c
    font-size: 13px

@media (max-width: 460px)
    .container
        width: 90%
        padding: 20px
        margin: 0 0 5px 0
</style>