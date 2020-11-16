<template>
    <div id="formBlock">
        <div class="routerLink">
            <router-link to="/" >
                <button>back</button>
            </router-link>
            <hr>
        </div>
        <form id="form" @submit.prevent="onSubmit">
            <div class="container">
                <h2>Личные данные</h2>
                <Input
                        v-for="personalData of personalData"
                        :key="personalData.name"
                        v-bind:inputData="personalData"
                        v-bind:formData="formData"
                        v-bind:v="$v"
                />
            </div>
            <div class="container">
                <h2>Адрес</h2>
                <Input
                        v-for="address of address"
                        :key="address.name"
                        v-bind:inputData="address"
                        v-bind:formData="formData"
                        v-bind:v="$v"
                />
            </div>
            <div class="container">
                <h2>Документ</h2>
                <Input
                        v-for="document of document"
                        :key="document.name"
                        v-bind:inputData="document"
                        v-bind:formData="formData"
                        v-bind:v="$v"

                />
            </div>
            <button class="formButton" type="submit">ок</button>
        </form>
        <div id="successful" ref="successful">
            <p>Новый клиент успешно создан!</p>
            <button class="formButton"  type="submit" v-on:click="closeWindow">Выход</button>
        </div>
    </div>
</template>

<script>
import { required } from 'vuelidate/lib/validators'
import Input from '../components/Form/Input'

export default {
    data() {
        return {
            formData: {
                lastName: '',
                firstName: '',
                surname: '',
				dateOfBirth: '',
                phoneNumber: '7',
                gender: '',
                clientGroup: [],
                doctor: '',
                dontSendSMS: false,
                postCode: '',
                country: '',
                region: '',
                city: '',
                street: '',
                house: '',
                typeDocument: '',
                series: '',
                number: '',
                issued: '',
                dateOfIssue: ''
            },
			personalData: [
				{label: 'Фамилия', name: 'lastName', class: 'lastName', type: 'text', required: true},
				{label: 'Имя',name: 'firstName', class: 'name', type: 'text', required: true},
				{label: 'Отчество',name: 'surname', class: 'Surname', type: 'text'},
				{label: 'Дата рождения',name: 'dateOfBirth', class: 'dateOfBirth', type: 'Date', required: true},
				{label: 'Телефон',name: 'phoneNumber', class: 'phoneNumber', type: 'number'},
				{label: 'Пол',name: 'gender', class: 'gender', type: 'radio'},
				{label: 'Группа клиентов',name: 'clientGroup', class: 'clientGroup', type: 'm-select',
					options: ['VIP','Проблемные','ОМС'], required: true},
				{label: 'Лечащий врач',name: 'doctor', class: 'doctor', type: 'select',
					options: ['Иванов','Захаров','Чернышев']},
				{label: 'Не отправлять СМС',name: 'dontSendSMS', class: 'dontSendSMS', type: 'checkbox'}
			],
			address: [
				{label: 'Индекс',name: 'postCode', class: 'postCode', type: 'text'},
				{label: 'Страна',name: 'country', class: 'country', type: 'text'},
				{label: 'Область',name: 'region', class: 'region', type: 'text'},
				{label: 'Город',name: 'city', class: 'city', type: 'text', required: true},
				{label: 'Улица',name: 'street', class: 'street', type: 'text'},
				{label: 'Дом',name: 'house', class: 'house', type: 'text'}
			],
			document: [
				{label: 'Тип документа',name: 'typeDocument', class: 'typeDocument', type: 'select',
					options: ['Паспорт','Свидетельство о рождении','Водительское удостоверение'], required: true},
				{label: 'Сеоия',name: 'series', class: 'series', type: 'text'},
				{label: 'Номер',name: 'number', class: 'number', type: 'number'},
				{label: 'Выдан',name: 'issued', class: 'issued', type: 'text'},
				{label: 'Дата выдачи',name: 'dateOfIssue', class: 'dateOfIssue', type: 'Date', required: true}
			]
        }
    },
    validations: {
        formData: {
            lastName: { required },
			firstName: { required },
			dateOfBirth:{ required },
			phoneNumber: {
                length(value) {
                    return value.length === 11 && value[0] === '7'
                }
            },
			clientGroup: { required },
			city: { required },
			typeDocument: { required },
			dateOfIssue: { required },
			surname:{},
			gender:{},
			doctor:{},
			dontSendSMS:{},
			postCode:{},
			country:{},
			region:{},
			street:{},
			house:{},
			series:{},
			number:{},
			issued:{},
        }
    },
    components: { Input },
    methods: {
        onSubmit() {
			this.$v.formData.$touch()
			if (!this.$v.formData.$error) {
				const el = this.$refs.successful
				el.style.display = 'block'
			} else if(this.$v.formData.$error) {
				let reqItems = this.personalData.filter((e) => e.required)  // фильтруем элементы с полями required
                reqItems = reqItems.concat(this.address.filter((e) => e.required))
				reqItems = reqItems.concat(this.document.filter((e) => e.required))
                for(let i in reqItems) {
                    if(this.$v.formData[reqItems[i].name].$error) { // проверяем на наличие ошибок в валидации
						document.getElementById(reqItems[i].name).scrollIntoView({block: "center", behavior: "smooth"}) // показываем поле с ошибкой валидации
                    }

                }
            }
		},
        closeWindow() {
            const el = this.$refs.successful
			el.style.display = 'none';
		}
    }
}
</script>
<style scoped lang="sass">
#formBlock
    text-align: center
.container
    margin: 20px auto 0 auto
    width: 400px
    background-color: #ffff
    border-radius: 5px
    border: 1px solid #dce0e2
    text-align: left
    padding: 20px
.formButton
    cursor: pointer
    margin-top: 5px
    margin-bottom: 100px
    width: 120px
    height: 40px
    background-color: #ffff
    border-radius: 5px
    border: 1px solid #dce0e2
    font-weight: 700
    color: rgba(147,159,173,.84)
    font-size: 16px
    text-transform: uppercase
    line-height: .9375rem
    letter-spacing: 1.2px
    font-family: 'Jost', sans-serif
.formButton:focus
    outline: none
    border: 1px solid #29B0D9
div#successful p
    font-weight: 700
    color: rgba(147,159,173,.84)
    font-size: .8rem
    text-transform: uppercase
    line-height: .9375rem
    letter-spacing: 1.2px
#successful
    position: fixed
    top: 50%
    left: 50%
    margin: -150px 0 0 -180px
    display: none
    max-width: 300px
    max-height: 100px
    background-color: #ffff
    padding: 50px
    border-radius: 10px
    border: 1px solid #dce0e2
.routerLink
    width: 440px
    margin: 0 auto
.routerLink button
    cursor: pointer
.formButton:active
    transform: scale(0.98)
@media (max-width: 460px)
    .container
        width: 90%
        padding: 20px
        margin: 0 0 5px 0
    #successful
        margin-left: -175px
</style>