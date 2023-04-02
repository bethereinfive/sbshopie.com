<template>


        <form @submit.prevent="login" style="padding-bottom: 70px" class="mt-5">

            <div style="background-color: #DAE0EE;" class="mx-3 p-3 rounded-4 text-center">
                <h1 class="fs-2 fw-bold mb-3" style="color: #0069BD;">প্রবেশ করুন</h1>
                <div class="bg-white border-white mb-3 mx-2 p-2 rounded-3">
                    <div class="border-bottom input-group pb-2">
                        <span class="bg-white border-0 input-group-text" id="basic-addon1"><i class="fa-solid fa-user"></i></span>
                        <input type="text" class="border-0 form-control" v-model="form.mobile" placeholder="অনুগ্রহ করে ব্যবহারকারীর মোবাইল নাম্বার প্রদান করুনঃ"  aria-describedby="basic-addon1">
                    </div>
                    <div class="input-group pb-2">
                        <span class="bg-white border-0 input-group-text" id="basic-addon1"><i class="fa-solid fa-lock"></i></span>
                        <input type="password" class="border-0 form-control"  v-model="form.password" placeholder=" পাসওয়ার্ড প্রদান করুনঃ" aria-describedby="basic-addon1">
                    </div>

                </div>

                <div class="text-end"> <router-link :to="{name:'register'}" class=" mb-2 mx-2 rounded-2 text-primary" style="border-color: #0069BD;">নিবন্ধন </router-link></div>

                <div class="mx-2">
                    <button type="button" class="btn w-100 text-white rounded-4"  v-if="loadLogin" style="background-color: #0069BD;">অপেক্ষা করুন....</button>
                    <button type="submit" class="btn w-100 text-white rounded-4" v-else style="background-color: #0069BD;">প্রবেশ করুন</button>

                </div>
            </div>

        </form>









</template>

<script>
export default {
    created() {
        this.addcountry();
        this.countryList();
    },

    data() {
        return {
            emailLogin: "",
            passwordLogin: "",
            emailReg: "",
            passwordReg: "",
            confirmReg: "",
            emptyFields: false,
            form: {
                mobile: "",
                password: "",
            },
            country: "+880",
            mobileCode: "",
            errors: {},
            codes: {},
            loadLogin: false,
        };
    },
    methods: {
        async countryList() {
            var res = await this.callApi(
                "get",
                `${this.$asseturl}CountryCodes.json`,
                []
            );
            // console.log(res)
            this.codes = res.data;
        },
        async addcountry() {
            this.mobileCode = this.country;
        },

        login() {
            this.loadLogin = true;

            if (this.form.mobile == "" || this.form.password == "") {
                this.emptyFields = true;
            } else {
                axios
                    .post("/login", this.form)
                    .then((res) => {
                        if (res.data == 0) {
                            Notification.customError(
                                "Please Enter Valid Phone Number and Password"
                            );
                            this.loadLogin = false;
                        } else if (res.data == 422) {
                            Notification.customError(
                                "Your Account Has Been Banded!"
                            );
                            this.loadLogin = false;
                        } else if (res.data == 444) {
                            Notification.customError(
                                "You Cant Login Multiple account same device!"
                            );
                            this.loadLogin = false;
                            localStorage.setItem("dmdevice", 1);
                        } else {
                            localStorage.setItem("dmdevice", 1);
                            User.responseAfterLogin(res);
                            if (res.data.role == "admin") {
                                window.location.href = "/dashboard/adddmin";
                            } else {
                                window.location.href = "/dashboard/user";
                            }

                            Notification.customSuccess(
                                "Signed in successfully Complete"
                            );

                            // this.$router.push({name: 'home'})
                            // window.location.href = '/dashboard'
                        }
                    })
                    .catch(
                        (error) => (this.errors = error.response.data.errors)
                    );
            }
        },

        blur(id) {
            const child = document.getElementById(id);
            if (this.form[id] == "") {
                child.parentNode.classList.remove("blursuccess");
                child.parentNode.classList.add("blurerror");
            } else {
                child.parentNode.classList.remove("blurerror");
                child.parentNode.classList.add("blursuccess");
            }
        },
    },
};
</script>

<style lang="css" scoped>
.languagechange {
    width: 100px;
    float: right;
}

section.vh-100 {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 100%;
}

.blurerror input {
    border: 1px solid red !important;
}

.blurerror label {
    color: red !important;
}

.blursuccess input {
    border: 1px solid green !important;
}

.blursuccess label {
    color: green !important;
}

.divider:after,
.divider:before {
    content: "";
    flex: 1;
    height: 1px;
    background: #eee;
}

*,
*:focus {
    outline: none;
}

/* .form{
  width: 500px;
  margin: 0 auto;
  margin-top: 150px;
  font-family: sans-serif;
  background: #fff
} */
.form-item {
    position: relative;
    margin-bottom: 15px;
}

.form-item input {
    display: block;
    width: 100%;
    height: 40px;
    background: transparent;
    border: solid 1px #ccc;
    transition: all 0.3s ease;
    padding: 0 15px;
}

.form-item input:focus {
    border-color: blue;
}

.form-item label {
    position: absolute;
    cursor: text;
    z-index: 2;
    top: 13px;
    left: 10px;
    font-size: 12px;
    font-weight: bold;
    background: #fff;
    padding: 0 10px;
    color: #999;
    transition: all 0.3s ease;
}

.form-item input:focus + label,
.form-item input:valid + label {
    font-size: 11px;
    top: -5px;
}

.form-item input:focus + label {
    color: blue;
}
</style>
