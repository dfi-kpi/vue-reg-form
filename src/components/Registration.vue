<template>
    <div class="align-items-center py-4 bg-body-tertiary w-100 m-auto">
        <main>
            <div class="form-signin">
                <form @submit.prevent="submitForm">
                    <h1 class="h3 mb-3 fw-normal text-center">Форма реєстрації</h1>
                    <div class="form-group">
                        <label>Прізвище</label>
                        <input v-model="lastName" @input="validateInput('lastName')" type="text" class="form-control" placeholder="" required>
                        <small id="lastName-error" class="error-message">{{ errors.lastName }}</small>
                    </div>
                    <div class="form-group">
                        <label>Ім'я</label>
                        <input v-model="firstName" @input="validateInput('firstName')" type="text" class="form-control" placeholder="" required>
                        <small id="firstName-error" class="error-message">{{ errors.firstName }}</small>
                    </div>
                    <div class="form-group">
                        <label>По батькові</label>
                        <input v-model="middleName" @input="validateInput('middleName')" type="text" class="form-control" placeholder="" required>
                        <small id="middleName-error" class="error-message">{{ errors.middleName }}</small>
                    </div>
                    <div class="form-group">
                        <label>Дата народження</label>
                        <input v-model="birth" @input="validateInput('birth')" type="date" class="form-control" required>
                        <small id="birth-error" class="error-message">{{ errors.birth }}</small>
                    </div>
                    <div class="form-group">
                        <label>Пошта</label>
                        <input v-model="email" @input="validateInput('email')" type="email" class="form-control" placeholder="" required>
                        <small id="email-error" class="error-message">{{ errors.email }}</small>
                    </div>
                    <div class="form-group">
                        <label>Стать</label>
                        <div class="justify-content-between" style="display: flex;">
                            <div class="form-check">
                                <input id="male" name="sex" type="radio" class="form-check-input" value="Чоловік" v-model="sex" required>
                                <label class="form-check-label" for="male">Чоловік</label>
                            </div>
                            <div class="form-check">
                                <input id="female" name="sex" type="radio" class="form-check-input" value="Жінка" v-model="sex" required>
                                <label class="form-check-label" for="female">Жінка</label>
                            </div>
                            <div class="form-check">
                                <input id="unknown" name="sex" type="radio" class="form-check-input" value="Не скажу" v-model="sex" required checked>
                                <label class="form-check-label" for="unknown">Не скажу</label>
                            </div>
                        </div>
                    </div>
                    <div class="form-group">
                        <label>Номер телефону</label>
                        <input ref="phoneInput" v-model="phone" @input="validateInput('phone')" type="text" placeholder=" " class="form-control" required>
                        <small id="phone-error" class="error-message">{{ errors.phone }}</small>
                    </div>
                    <div class="form-group">
                        <label>Фото профілю</label>
                        <input @change="handleFileChange" type="file" class="form-control" accept=".png, .jpg, .gif, .jpeg" required>
                        <small id="photo-error" class="error-message">{{ errors.photo }}</small>
                    </div>
                    <div class="form-group">
                        <label>Група</label>
                        <select v-model="group" @input="validateInput('group')" name="groups" id="groups" class="form-control" placeholder="" required>
                            <option value="dl51">ДЛ-51</option>
                            <option value="mf15">МФ-15</option>
                            <option value="pa81">ПА-81</option>
                        </select>
                        <small id="group-error" class="error-message">{{ errors.group }}</small>
                    </div>
                    <div class="form-group">
                        <label>Пароль</label>
                        <input v-model="password" @input="validateInput('password')" type="password" class="form-control" placeholder="" required>
                        <small id="password-error" class="error-message">{{ errors.password }}</small>
                    </div>
                    <button class="btn btn-success w-100 py-2 mt-5" :disabled="!allowReg" type="submit">Зареєструватися</button>
                    <button class="btn btn-danger w-100 py-2 mt-2" type="reset">Скинути</button>
                </form>
            </div>
            <div class="w-75 m-auto mt-5 pb-5">
                <div class="text-center m-3 mx-5">
                    <button @click="deleteRows" class="btn btn-md btn-danger mx-4">Видалити</button>
                    <button @click="duplicateRows" class="btn btn-md btn-dark mx-4">Продублювати</button>
                </div>
                <table class="table table-striped table-bordered table-hover">
                    <thead>
                        <tr>
                            <th>#</th>
                            <th>Ім'я</th>
                            <th>Прізвище</th>
                            <th>По батькові</th>
                            <th>Дата народження</th>
                            <th>Пошта</th>
                            <th>Стать</th>
                            <th>Номер телефону</th>
                            <th>Фото профілю</th>
                            <th>Група</th>
                            <th>Пароль</th>
                        </tr>
                    </thead>
                    <tbody id="table-data">
                        <!-- PLACEHOLDER -->
                        <tr v-for="person in persons" :key="person.id">
                            <td style="width: 5px;"><input type="checkbox"></td>
                            <td>{{ person.firstName }}</td>
                            <td>{{ person.lastName }}</td>
                            <td>{{ person.middleName }}</td>
                            <td>{{ person.birth }}</td>
                            <td>{{ person.email }}</td>
                            <td>{{ person.sex }}</td>
                            <td>{{ person.phone }}</td>
                            <td>{{ person.photo }}</td>
                            <td>{{ person.group }}</td>
                            <td>{{ person.password }}</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </main>
    </div>
</template>

<script>
    import Inputmask from 'inputmask';

    export default {
        name: 'HelloWorld',
        data() {
            return {
                lastName: '',
                firstName: '',
                middleName: '',
                birth: '',
                phone: '',
                group: '',
                sex: 'unknown',
                email: '',
                photo: '',
                password: '',
                regex: {
                    lastName: /^[A-Z][a-zA-Z'-]{1,}$/,
                    firstName: /^[A-Z][a-zA-Z'-]{1,}$/,
                    middleName: /^[A-Z][a-zA-Z'-]{1,}$/,
                    email: /^\w+@[a-zA-Z_]+?\.[a-zA-Z]{2,}$/,
                    birth: /^(?:\d{4}-(0[1-9]|1[0-2])-(0[1-9]|[12][0-9]|3[01]))$/,
                    password: /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/,
                    phone: /^\+38\(0[1-9][0-9]\)[0-9]{3}-[0-9]{2}-[0-9]{2}$/
                },
                allowReg: false,
                errors: {
                    lastName: '',
                    firstName: '',
                    middleName: '',
                    birth: '',
                    email: '',
                    password: '',
                    phone: ''
                },
                persons: [
                    {
                        id: 1,
                        firstName: 'Abc',
                        lastName: 'Abc',
                        middleName: 'Abc',
                        birth: '0444-04-04',
                        email: 'super@mail.io',
                        sex: 'Жінка',
                        phone: '+38(042)352-34-23',
                        photo: 'C:\\fakepath\\marieke-douma-gloomwood.jpg',
                        group: 'mf15',
                        password: 'Kyter133!'
                    },
                    {
                        id: 2,
                        firstName: 'Ivan',
                        lastName: 'Prep',
                        middleName: 'Vital',
                        birth: '2005-04-14',
                        email: 'sdfsdfsdfsdfsfsfd@dfdsf.rr',
                        sex: 'Чоловік',
                        phone: '+38(055)555-55-55',
                        photo: 'C:\\fakepath\\marieke-douma-gloomwood.jpg',
                        group: 'pa81',
                        password: 'Kyter133!'
                    }
                ]
            };
        },
        methods: {
            handleFileChange(event) {
                const selectedFile = event.target.files[0];

                if (selectedFile) {
                    this.photo = selectedFile.name;
                } else {
                    this.photo = "";
                }
            },
            validateInput(id) {
                const value = this[id];
                let errorMessage = '';

                if (id === "birth") {
                    let today = new Date();
                    let date = `${today.getFullYear()}-${(today.getMonth() + 1).toString().padStart(2, '0')}-${today.getDate().toString().padStart(2, '0')}`;
                    if (value > date) {
                        errorMessage = "Введіть правильну дату!";
                    }
                } else if (this.regex[id]) {
                    if (!this.regex[id].test(value)) {
                        switch (id) {
                            case "firstName":
                                errorMessage = "Ім'я має починатися з великої літери та мати не менше двох символів! Дозволені лише латинські літери!";
                                break;
                            case "middleName":
                                errorMessage = "Побатькові має починатися з великої літери та мати не менше двох символів! Дозволені лише латинські літери!";
                                break;
                            case "lastName":
                                errorMessage = "Прізвище має починатися з великої літери та мати не менше двох символів! Дозволені лише латинські літери!";
                                break;
                            case "email":
                                errorMessage = "Введена пошта не відповідає існуючим стандартам!";
                                break;
                            case "password":
                                errorMessage = "Довжина паролю не менше 8 символів! Хоча б одна велика літера, одна маленька літера, одна цифра, одни спец символ!";
                                break;
                            case "phone":
                                errorMessage = "Введіть Ваш дійсний номер телефону!";
                                break;
                        }
                    }
                }

                this.errors[id] = errorMessage;

                this.allowReg = !Object.values(this.errors).some(error => error !== '');
                console.log(this.allowReg);
            },
            addToTable: function () {
                if (this.allowReg) {
                    this.persons.push({
                        lastName: this.lastName,
                        firstName: this.firstName,
                        middleName: this.middleName,
                        birth: this.birth,
                        email: this.email,
                        sex: this.sex,
                        phone: this.phone,
                        photo: this.photo,
                        group: this.group,
                        password: this.password,
                    });
                }
            },
            submitForm() {
                // console.log("lastName: " + this.lastName);
                // console.log("firstName: " + this.firstName);
                // console.log("middleName: " + this.middleName);
                // console.log("birth: " + this.birth);
                // console.log("email: " + this.email);
                // console.log("sex: " + this.sex);
                // console.log("phone: " + this.phone);
                // console.log("photo: " + this.photo);
                // console.log("group: " + this.group);
                // console.log("password: " + this.password);
                
                if (this.allowReg) {
                    this.addToTable();
                    
                    this.lastName = "";
                    this.firstName = "";
                    this.middleName = "";
                    this.birth = "";
                    this.email = "";
                    this.sex = "unknown";
                    this.phone = "";
                    this.photo = "";
                    this.group = "";
                    this.password = "";
                }
            },
            deleteRows: function () {
                    let checked = Array.from(document.querySelectorAll('#table-data input:checked'));
                    checked.forEach(it => {
                        it.parentNode.parentNode.remove();
                    });
            },
            duplicateRows: function () {
                let checked = Array.from(document.querySelectorAll('#table-data input:checked'));
                checked.forEach(it => {
                    document.getElementById('table-data').innerHTML += it.parentNode.parentNode.outerHTML;
                });
            }
        },
        mounted() {
            Inputmask({mask: '+38(099)999-99-99'}).mask(this.$refs.phoneInput);
        }
    };
</script>

<style>
    html, body {
        height: 100% !important;
    }

    .svg-white {
        fill: white;
    }

    .icon-text-wrapper {
        display: flex;
        margin-top: 0;
        flex-direction: column;
        align-items: center;
    }

    .icon-text-wrapper i {
        font-size: 24px;
    }

    @keyframes ldio-ycibxgb3u0a {
        0% { opacity: 1 }
        50% { opacity: .5 }
        100% { opacity: 1 }
    }
    .ldio-ycibxgb3u0a div {
        position: absolute;
        width: 26.7px;
        height: 106.8px;
        top: 80.1px;
        animation: ldio-ycibxgb3u0a 1s cubic-bezier(0.5,0,0.5,1) infinite;
    }.ldio-ycibxgb3u0a div:nth-child(1) {
        transform: translate(40.05px,0);
        background: #3c1642;
        animation-delay: -0.6s;
    }.ldio-ycibxgb3u0a div:nth-child(2) {
        transform: translate(93.45px,0);
        background: #086375;
        animation-delay: -0.4s;
    }.ldio-ycibxgb3u0a div:nth-child(3) {
        transform: translate(146.85px,0);
        background: #1dd3b0;
        animation-delay: -0.2s;
    }.ldio-ycibxgb3u0a div:nth-child(4) {
        transform: translate(200.25px,0);
        background: #affc41;
        animation-delay: -1s;
    }
    .loadingio-spinner-bars-tsev48yigpj {
        width: 267px;
        height: 267px;
        display: inline-block;
        overflow: hidden;
        background: rgba(NaN, NaN, NaN, 0);
    }
    .ldio-ycibxgb3u0a {
        width: 100%;
        height: 100%;
        position: relative;
        transform: translateZ(0) scale(1);
        backface-visibility: hidden;
        transform-origin: 0 0; /* see note above */
    }
    .ldio-ycibxgb3u0a div { box-sizing: content-box; }


    .bd-placeholder-img {
        font-size: 1.125rem;
        text-anchor: middle;
        -webkit-user-select: none;
        -moz-user-select: none;
        user-select: none;
    }

    @media (min-width: 768px) {
        .bd-placeholder-img-lg {
        font-size: 3.5rem;
        }
    }







    /* NEXT */


    /* GLOBAL STYLES
    -------------------------------------------------- */
    /* Padding below the footer and lighter body text */
    
    
    /* CUSTOMIZE THE CAROUSEL
    -------------------------------------------------- */
    
    /* Carousel base class */
    .carousel {
        margin-bottom: 4rem;
    }
    /* Since positioning the image, we need to help out the caption */
    .carousel-caption {
        bottom: 3rem;
        z-index: 10;
    }
    
    /* Declare heights because of positioning of img element */
    .carousel-item {
        height: 32rem;
    }
    
    
    /* MARKETING CONTENT
    -------------------------------------------------- */
    
    /* Center align the text within the three columns below the carousel */
    .marketing .col-lg-4 {
        margin-bottom: 1.5rem;
        text-align: center;
    }
    /* rtl:begin:ignore */
    .marketing .col-lg-4 p {
        margin-right: .75rem;
        margin-left: .75rem;
    }
    /* rtl:end:ignore */
    
    
    /* Featurettes
    ------------------------- */
    
    .featurette-divider {
        margin: 5rem 0; /* Space out the Bootstrap <hr> more */
    }
    
    /* Thin out the marketing headings */
    /* rtl:begin:remove */
    .featurette-heading {
        letter-spacing: -.05rem;
    }
    
    /* rtl:end:remove */
    
    /* RESPONSIVE CSS
    -------------------------------------------------- */
    
    @media (min-width: 40em) {
        /* Bump up size of carousel content */
        .carousel-caption p {
        margin-bottom: 1.25rem;
        font-size: 1.25rem;
        line-height: 1.4;
        }
    
        .featurette-heading {
        font-size: 50px;
        }
    }
    
    @media (min-width: 62em) {
        .featurette-heading {
        margin-top: 7rem;
        }
    }












    .form-signin {
        margin: 0px auto;
        max-width: 330px;
        padding: 1rem;
    }

    .form-signin .form-floating:focus-within {
        z-index: 2;
    }

    .form-signin *[position="top"] {
        margin-bottom: -1px;
        border-bottom-right-radius: 0;
        border-bottom-left-radius: 0;
    }

    .form-signin *[position="middle"] {
        margin-bottom: -1px;
        border-bottom-right-radius: 0;
        border-bottom-left-radius: 0;
        border-top-left-radius: 0;
        border-top-right-radius: 0;
    }

    .form-signin *[position="bottom"] {
        margin-bottom: 10px;
        border-top-left-radius: 0;
        border-top-right-radius: 0;
    }










    .my_form {
    max-width: 700px;
    margin: 20px auto;
    text-align: left;
    }
    .form-group {
    margin-top: 5px;
    }



    table {
    border-collapse: collapse;
    border-spacing: 0;
    width: 100%;
    border: 1px solid #ddd;
    }
    th {
    font-style: italic;
    font-weight: bold;
    }
    .icon-col a {
    display: none;
    }
    .table tr:hover .icon-col a {
    display: inline;
    justify-content: space-evenly;
    }








    .error-message {
    color: crimson; 
    font-weight: bold;
    }
</style>