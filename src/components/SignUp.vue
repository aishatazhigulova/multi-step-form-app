<template>
    <div class="sign-up">
        <div v-if="this.stage !== RESULT_STAGE" class="sign-up__stages scalingUp">
            <base-card>
                <div class="sign-up__form">
                    <h1 class="heading-primary">Sign up</h1>
                    <div class="form">
                        <div
                            v-for="(input, index) in signUpInputStages"
                            :key="input.id"
                            :style="{marginLeft: index === 0 ? formBoxMarginLeft : '0px'}"
                            class="form__form-box"
                        >
                            <label
                                :for="input.id"
                                class="form-box__label"
                            >
                                {{ input.label }}
                            </label>
                            <input
                                :id="input.id"
                                v-model.trim="enteredData[input.enteredDataKey]"
                                :class="{'invalid-input': formErrorMessage }"
                                :placeholder="input.placeholder"
                                :type="input.type"
                                class="form-box__input"
                                @input="getCorrectPhoneNumberInput"
                                @keyup.prevent="nextByEnter"
                            />
                        </div>
                    </div>
                    <p v-if="formErrorMessage" class="message">{{ formErrorMessage }}</p>

                    <div class="sign-up__bottom">
                        <button
                            v-if="this.stage > FIRST_NAME_STAGE"
                            class="btn"
                            @click="back"
                            v-text="'Back'"
                        />
                        <button
                            class="btn btn-left"
                            @click="next"
                            v-text="stage < RESULT_STAGE ? 'Next' : 'Submit'"
                        />
                    </div>
                </div>
            </base-card>
        </div>
        <div v-else class="signup__result ">
            <loading-gif v-if="isResultLoading"></loading-gif>
            <base-card v-else class="scalingUp">
                <div class="result">
                    <div class="result__content">

                        <h1 class="result__heading heading-primary">Submitted</h1>

                        <div class="result__list">
                            <div class="result__item">
                                <div class="result__title">First name:</div>
                                <div class="result__value">{{ enteredData.firstName }}</div>
                            </div>

                            <div class="result__item">
                                <div class="result__title">Last name:</div>
                                <div class="result__value">{{ enteredData.lastName }}</div>
                            </div>

                            <div class="result__item">
                                <div class="result__title">E-mail:</div>
                                <div class="result__value">{{ enteredData.email }}</div>
                            </div>

                            <div class="result__item">
                                <div class="result__title">Phone number:</div>
                                <div class="result__value">{{ enteredData.phoneNumber }}</div>
                            </div>
                        </div>

                        <base-button class="btn-wide" @click="restartSignUp">Sign up again</base-button>
                    </div>
                </div>
            </base-card>
        </div>

    </div>
</template>


<script>

import LoadingGif from "../components/LoadingGif";
import BaseButton from "../components/BaseButton";
import BaseCard from "../components/BaseCard";

export default {
    components: {
        BaseCard,
        BaseButton,
        LoadingGif,
    },
    data() {
        return {
            // some comments explaining what the fuck these variables mean
            // Sign up form has 4 steps: firstname - 0, lastname - 1 , email - 2, phone number - 3
            // Last step is our final result
            FIRST_NAME_STAGE: 0,
            LAST_NAME_STAGE: 1,
            EMAIL_STAGE: 2,
            PHONE_NUMBER_STAGE: 3,
            RESULT_STAGE: 4,

            // By incrementing / decrementing stage we can switch between form inputs
            stage: 0,

            enteredData: {
                firstName: '',
                lastName: '',
                email: '',
                phoneNumber: ''
            },

            // enteredDataKey - we can set v-model dynamically by accessing key
            signUpInputStages: [
                {
                    id: 'first-name',
                    type: 'text',
                    label: 'First name',
                    enteredDataKey: 'firstName',
                    placeholder: ' '

                },
                {
                    id: 'last-name',
                    type: 'text',
                    label: 'Last name',
                    enteredDataKey: 'lastName',
                    placeholder: ' ',
                },
                {
                    id: 'email',
                    type: 'email',
                    label: 'Email',
                    enteredDataKey: 'email',
                    placeholder: ' ',
                },
                {
                    id: 'phone-number',
                    type: 'tel',
                    label: 'Phone number',
                    enteredDataKey: 'phoneNumber',
                    placeholder: '+(7__) ___ - __ - __'
                },
            ],

            formErrorMessage: '',

            //this is to imitate server response wait state
            isResultLoading: true,

            // Regex is hard to read. So I provided comments
            regexPatterns: {
                // first name should accept only letters and hyphens
                //letters latin cyrillic letters as well
                firstName: /^[a-zA-Zа-яА-ЯёЁ'][a-zA-Z-а-яА-ЯёЁ' ]+[a-zA-Zа-яА-ЯёЁ']?$/,
                // first name should accept only letters and hyphens
                //letters latin cyrillic letters as well
                lastName: /^[a-zA-Zа-яА-ЯёЁ'][a-zA-Z-а-яА-ЯёЁ' ]+[a-zA-Zа-яА-ЯёЁ']?$/,
                email: /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/,
            }
        }
    },
    computed: {
        // this is for slider functionality
        // this method calculates margin left to set the next slide input
        formBoxMarginLeft() {
            return `-${25 * this.stage}%`
        },
    },
    methods: {
        submitData() {
            // this is to imitate server response waiting
            setTimeout(() => this.isResultLoading = false, 5000)
        },

        validateStageInput() {
            switch (this.stage) {
                case this.FIRST_NAME_STAGE:
                    if (!this.enteredData.firstName.length) {
                        return 'First name is required'
                    }

                    if (this.enteredData.firstName.length < 2) {
                        return 'First name must contain at least two letters'
                    }

                    if (!this.regexPatterns.firstName.test(this.enteredData.firstName)) {
                        return 'Please enter the correct first name'
                    }

                    return ''

                case this.LAST_NAME_STAGE:
                    if (!this.enteredData.lastName.length) {
                        return 'Last name is required'
                    }

                    if (this.enteredData.lastName.length < 2) {
                        return 'First name must contain at least two letters'
                    }

                    if (!this.regexPatterns.lastName.test(this.enteredData.lastName)) {
                        return 'Please enter the correct first email'
                    }

                    return ''

                case this.EMAIL_STAGE:
                    if (!this.enteredData.email.length) {
                        return 'Email is required'
                    }

                    if (!this.regexPatterns.email.test(this.enteredData.email)) {
                        return 'Please enter the correct email'
                    }

                    return ''

                case this.PHONE_NUMBER_STAGE:
                    if (!this.enteredData.phoneNumber.length) {
                        return 'Phone number is required'
                    }

                    if (this.enteredData.phoneNumber.replace(/\D/g, '').length === 12) {
                        return 'Please enter the correct phone number'
                    }

                    return ''
            }
        },

        getCorrectPhoneNumberInput() {
            const value = this.enteredData.phoneNumber.replace(/\D+/g, "");
            const numberLength = 11;

            let result = ''

            for (let i = 0; i < value.length && i < numberLength; i++) {
                switch (i) {
                    case 0:
                        result += "+7 (";
                        continue;
                    case 4:
                        result += ") ";
                        break;
                    case 7:
                        result += "-";
                        break;
                    case 9:
                        result += "-";
                        break;
                    default:
                        break;
                }
                result += value[i];
            }
            this.enteredData.phoneNumber = result;
        },


        back() {
            if (this.stage === this.FIRST_NAME_STAGE) {
                return
            }

            this.stage--
        },

        next() {
            let validationMessage = this.validateStageInput()

            if (validationMessage) {
                this.formErrorMessage = validationMessage
                setTimeout(() => this.formErrorMessage = '', 2000)
                return
            }

            this.formErrorMessage = ''
            this.stage++

            if (this.stage === this.PHONE_NUMBER_STAGE) {
                this.submitData()
            }
        },

        // Triggers next method on enter
        nextByEnter(e) {
            if (e.keyCode === 13) {
                this.next()
            }
        },

        restartSignUp() {
            this.stage = 0
            for (let data in this.enteredData) {
                this.enteredData[data] = ''
            }
        },
    }
}
</script>

<style scoped>

.sign-up__stages {
    width: 500px;
}

.sign-up__form {
    overflow: hidden;
    position: relative;
}

.sign-up .heading-primary {
    text-align: center;
}

.form {
    display: flex;
    width: 400%;
}

.form__form-box {
    width: 25%;
    transition: margin-left 0.4s ease-in-out;
}

.form-box__label {
    display: inline-block;
    margin-top: 0.5rem;
    margin-left: 1rem;
}

.form-box__input {
    border: none;
    padding: 0.6rem 1rem;
    border-radius: var(--br-small);
    width: 100%;
    height: 3rem;
    background-color: var(--color-primary-light);
    color: #ffffff;
}

.form-box__input:focus {
    outline: none;
    border: 1px solid var(--color-primary);
}

.message {
    color: red;
    margin-top: 0.5rem;
    margin-bottom: 0.5rem;
    margin-left: 1rem;
    transition: all 1s ease-out;
}

.sign-up__bottom {
    margin-top: 1rem;
    display: flex;
    justify-content: space-between;
}

.invalid-input {
    border: 1px solid var(--color-danger);
}

.btn-left {
    margin-left: auto;
}

.scalingUp {
    animation: scaleUp 0.4s;
}

.result__heading {
    text-align: center;
}

.result__list {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
}

.result__item {
    border-radius: var(--br-small);
    background-color: var(--color-primary-light);
    width: 48%;
    margin-bottom: 4%;
    padding: 0.6rem 1rem;
}

.result__title {
    font-weight: bold;
}

@keyframes scaleUp {
    0% {
        transform: scale(0.5);
    }
    100% {
        transform: scale(1);
    }
}


</style>
