<template>
    <div>
        <h1 v-if="Frontloader">loading....</h1>

<!--
        <nav data-v-161cf975="" class="navbar navbar-expand-sm bg-dark navbar-dark fixed-top" style="padding: 0px 0.5rem;">
                            <a data-v-161cf975="" href="#" class="navbar-brand"></a>
                            <ul data-v-161cf975="" class="navbar-nav">
                                <li data-v-161cf975="" class="nav-item">
                                    <a data-v-161cf975="" href="javascript:void(0)" class="nav-link"><span data-v-161cf975="" class="badge badge-pill badge-secondary" style="background: #0077C3; font-size: 15px;" @click="showbalance"><span v-if="balanceshow">{{ user.user.balance }} tk</span><span v-else>Check Balance</span></span></a>
                                </li>
                            </ul>
                        </nav> -->
 <main class="container" style="padding-top: 25px; margin-bottom: 40px;">

        <slot></slot>

    </main>
        <div data-v-3c5f5539="" data-v-161cf975="" class="rg-btn-container-foreground" style="--border-color: #212529; --icon-color: #212529; --background-color: #ffffff; --title-color: #212529; --badge-color: #212529;">
                            <div data-v-3c5f5539="" class="rg-btn-container">
                                <router-link :to="{name:'Authuser'}" data-v-3c5f5539="" class="rg-btn-item">
                                    <div data-v-3c5f5539="" class="rg-btn-border"></div>
                                    <div data-v-3c5f5539="" class="rg-btn-icon rg-btn-icon-active">
                                        <i class="fas fa-tachometer-alt"></i>
                                        <!---->
                                    </div>
                                    <div data-v-3c5f5539="" class="rg-btn-title rg-btn-title-active">
                                        Menu
                                    </div>
                                </router-link>
                            </div>
                            <div data-v-3c5f5539="" class="rg-btn-container">
                                <a :href="$asseturl+'SB_Shop.apk'" data-v-3c5f5539="" class="rg-btn-item">
                                    <div data-v-3c5f5539="" class=""></div>
                                    <div data-v-3c5f5539="" class="rg-btn-icon">
                                        <i class="fas fa-mobile-alt"></i>
                                        <!---->
                                    </div>
                                    <div data-v-3c5f5539="" class="rg-btn-title">
                                        App
                                    </div>
                                </a>
                            </div>
                            <div data-v-3c5f5539="" class="rg-btn-container">
                                <router-link :to="{name:'Usertask'}" data-v-3c5f5539="" class="rg-btn-item">
                                    <div data-v-3c5f5539="" class=""></div>
                                    <div data-v-3c5f5539="" class="rg-btn-icon">
                                        <i class="far fa-hand-pointer"></i>
                                        <!---->
                                    </div>
                                    <div data-v-3c5f5539="" class="rg-btn-title">
                                        Order
                                    </div>
                                </router-link>
                            </div>
                            <div data-v-3c5f5539="" class="rg-btn-container">
                                <router-link :to="{name:'support'}" data-v-3c5f5539="" class="rg-btn-item">
                                    <div data-v-3c5f5539="" class=""></div>
                                    <div data-v-3c5f5539="" class="rg-btn-icon">
                                        <i class="fas fa-hands-helping"></i>
                                        <!---->
                                    </div>
                                    <div data-v-3c5f5539="" class="rg-btn-title">
                                        Help
                                    </div>
                                </router-link>
                            </div>
                            <div data-v-3c5f5539="" class="rg-btn-container">
                                <router-link :to="{name:'Useraccount'}" data-v-3c5f5539="" class="rg-btn-item">
                                    <div data-v-3c5f5539="" class="rg-btn-border-deselect"></div>
                                    <div data-v-3c5f5539="" class="rg-btn-icon rg-btn-icon-deselect">
                                        <i class="fas fa-tasks"></i>
                                        <!---->
                                    </div>
                                    <div data-v-3c5f5539="" class="rg-btn-title">
                                        User
                                    </div>
                                </router-link>
                            </div>
                        </div>



    </div>
</template>
<script>
  export default {

    created() {
                this.createlink('frontend/css/bootstrap.min.css');
        this.createlink('frontend/style.css');
        this.createlink('frontend/css/style.css');
        this.createlink('frontend/css/font-awesome.min.css');

        this.createscript('frontend/js/bootstrap.bundle.min.js');
        this.createscript('frontend/js/bootstrap.min.js');
        this.checkbalance();
    },
    data(){
        return {

        }},
    watch: {
        '$route': {
            handler(newValue, oldValue) {
                window.scrollTo(0,0);
                this.balanceshow = false
            },
            deep: true
        }
    },
    mounted() {


    },
    methods: {




        createlink(url=''){
            let recaptchaScript = document.createElement('link')
            recaptchaScript.setAttribute('href', this.$asseturl+url)
            recaptchaScript.setAttribute('rel', 'stylesheet')
            document.head.appendChild(recaptchaScript)
        },
        createscript(url=''){
            let recaptchaScript = document.createElement('script')
            recaptchaScript.setAttribute('src', this.$asseturl+url)
            document.body.appendChild(recaptchaScript)
        }
    }
  }
</script>
<style>
.col-6.amount_item span {
  border: 2px solid  #e831b8;
  display: block;
  text-align: center;
  margin: 6px 0px;
  padding: 4px 3px;
  cursor: pointer;
}
input.money-btn {
    border: 1px solid red;
    color: red !important;
}
button.money-btn {
    padding: 1px 20px;
    margin: 11px 0px;
    font-size: 23px;
    background: #168080;
    color: white;
    width: 100%;
    border-radius: 21px;
}
section.intro.profileBg.mb-5,.layout-content {
    width: 90%;
    margin: 42px auto;
}
section.intro.profileBg.mb-5 {
    padding: 35px 0px;
}
</style>
