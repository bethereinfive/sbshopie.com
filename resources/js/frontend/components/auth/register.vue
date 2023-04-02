<template>

        <form @submit.prevent='register' style="padding-bottom: 70px;" class=" py-5">

<div style="background-color: #DAE0EE;" class="mx-3 p-3 rounded-4 text-center">
    <h1 class="fs-2 fw-bold mb-3" style="color: #0069BD;">নিবন্ধন করুন</h1>
    <div class="bg-white border-white mb-3 mx-2 p-2 rounded-3">
        <div class="border-bottom input-group pb-2">
            <span class="bg-white border-0 input-group-text" id="basic-addon1"><i class="fa-solid fa-user"></i></span>
            <input type="tel" v-model="form.mobile" class="border-0 form-control" placeholder="অনুগ্রহ করে ব্যবহারকারীর মোবাইল নাম্বার প্রদান করুনঃ"  aria-describedby="basic-addon1">
          </div>
        <div class="border-bottom input-group pb-2">
            <span class="bg-white border-0 input-group-text" id="basic-addon1"><i class="fa-solid fa-lock"></i></span>
            <input type="password" v-model="form.password" class="border-0 form-control" placeholder=" পাসওয়ার্ড ইনপুট করুনঃ" aria-describedby="basic-addon1">
          </div>
        <div class="border-bottom  input-group pb-2">
            <span class="bg-white border-0 input-group-text" id="basic-addon1"><i class="fa-solid fa-lock"></i></span>
            <input type="password" v-model="form.password_confirmation" class="border-0 form-control" placeholder=" পাসওয়ার্ড নিশ্চিত করুনঃ" aria-describedby="basic-addon1">
          </div>
        <div class="input-group pb-2">
            <span class="bg-white border-0 input-group-text"  id="basic-addon1"><i class="fa-solid fa-person-circle-check"></i></span>
            <input type="text"  v-model="form.ref_by" class="border-0 form-control" placeholder="" aria-describedby="basic-addon1" @input="refercheck" required>
          </div>

    </div>

   <div class="mx-2 mb-4">
    <button type="submit" class="btn w-100 text-white rounded-4" style="background-color: #E71B23;">নিবন্ধন</button>
   </div>

   <div class="mb-2 mx-3 text-start">
    <a href="#" style="color: #168080d0;">ইতিমধ্যে একাউন্ট আছে? </a>
   </div>
   <div class="mx-2">
    <button type="button" class="btn w-100 text-white rounded-4" style="background-color: #0069BD;"><router-link class="text-white" :to="{name:'/login'}">প্রবেশ করুন</router-link></button>

   </div>
</div>

</form>













</template>
<script>

export default {
    created() {
        this.genaratedCaptcha = Math.random().toString(36).substring(2, 6 + 2);
        // if (User.loggedIn()) {
        //     this.$router.push({ name: 'home' })
        // }
        // localStorage.setItem('dmdevice',1)
        // console.log(this.$route.query.ref);
        if (this.$route.query.ref) {
            this.form.ref_by = this.$route.query.ref;
            this.refer = 1
        } else {
            this.form.ref_by = 'dSbeVOj'
            this.refercheck();
        }
        this.form.country = '+880'
        this.countryList();
        this.addcountry()


        this.form.username = this.randomLetter(7);
        this.usernamecheck();



    },
    data() {
        return {


            btndis: true,
            captcha: '',
            genaratedCaptcha: '',
            mobileCode: null,
            form: {
                country: null,
                name: 'New User',
                username: null,
                mobile: null,
                password: null,
                password_confirmation: null,
                withdrawpass: '123456',
                ref_by: null,
            },
            usernameMatch: 1,
            refer: 0,
            errors: {},
            codes: {},
            showPassword: true,
            CshowPassword: true,
            WshowPassword: true,
        }
    },
    methods: {
        // setLang(){
        //     localStorage.setItem('language',this.$i18n.locale)
        // },

        randomLetter(length) {
                let result = '';
                const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
                const charactersLength = characters.length;
                let counter = 0;
                while (counter < length) {
                result += characters.charAt(Math.floor(Math.random() * charactersLength));
                counter += 1;
                }
                return result;
            },

        async usernamecheck() {
            if (this.form.username == '') {
                this.usernameMatch = 0;
            } else {
                var res = await this.callApi('get', `/api/count/username/check?username=${this.form.username}`, []);
                if (res.data == 0) {
                    this.usernameMatch = 2
                } else {
                    this.usernameMatch = 1
                }
            }
        },
        async countryList() {
            var res = await this.callApi('get', `${this.$asseturl}CountryCodes.json`, []);
            // console.log(res)
            this.codes = res.data
        },
        async addcountry() {
            // this.form.mobile = this.form.country
            this.mobileCode = this.form.country
        },
        async refercheck() {
            if (this.form.ref_by == '') {
                this.refer = 0;
            } else {
                var res = await this.callApi('get', `/api/count/username/check?username=${this.form.ref_by}`, []);
                if (res.data == 0) {
                    this.refer = 2
                } else {
                    this.refer = 1
                }
            }
        },
        register() {
            this.usernamecheck();
            // if(localStorage.getItem('dmdevice')){
            //     Notification.customError(`This device has already have an account!`);
            // }else{
            // if (this.genaratedCaptcha === this.captcha) {
                if(this.usernameMatch!=2){
                    Notification.customError('Something want wrong.Please Try again');
                    this.form.username = this.randomLetter(7);
                }else{
                if (this.refer != 1) {
                    Notification.customError('Opps,Refer code is Invalid');
                } else {
                    if (this.form.password === this.form.password_confirmation) {
                        axios.post('api/auth/register', this.form)
                            .then(res => {
                                if (res.data == 422) {
                                    Notification.customError('This Phone Number Already Exist');
                                } else if (res.data == 444) {
                                    Notification.customError(`This device has already have an account!`);
                                    localStorage.setItem('dmdevice', 1)
                                } else {
                                    // console.log(res)
                                    if (res.status == 201) {
                                        Notification.customSuccess('Registration Success');
                                        localStorage.setItem('dmdevice', 1)
                                        this.$router.push({ name: '/login' })
                                    } else {
                                        Notification.customError('Something want wrong. Please Try again or contact with admin');
                                    }
                                    // User.responseAfterLogin(res)
                                }
                                // console.log(res.data)
                                // User.responseAfterLogin(res)
                            })
                            .catch(error => this.errors = error.response.data.errors)
                    } else {
                        Notification.customError('Password and Confirm password does not match');
                    }
                }
                }
            // } else {
            //     Notification.customError('Captcha does not match!');
            // }
            // }
        }
    }
}
</script>
<style lang="css" scoped>

.languagechange {
    width: 100px;
    float: right;
}
button.button {
    padding: 7px 5px;
}
</style>
