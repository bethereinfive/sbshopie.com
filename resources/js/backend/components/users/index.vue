<template>
  <div>






                        <!---->
                        <div data-v-083876c0="">
                         <div class="top-banner">
  <img :src="$asseturl+'header.jpeg'" alt="" srcset="">
                         </div>
                            <div data-v-083876c0="" class="container2 mt-2" style="box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px; width: 90%; border-radius: 10px;">
                                <div data-v-083876c0="" class="row">

                                    <div data-v-083876c0="" class="col-md-3 col-3 mb-3 mt-3 text-center">
                                        <router-link :to="{name:'Recharge'}" class="">
                                            <i class="fa-solid fa-circle-dollar-to-slot" style="font-size: 25px;color: #C89831;"></i> <br>


                                            <!-- <img data-v-083876c0="" :src="$asseturl+'frontend/img/recharge.jpeg'" width="40px" /> <br data-v-083876c0="" /> -->
                                            <span data-v-083876c0="" class="font-17">Top Up</span>
                                        </router-link>
                                    </div>



                                    <div data-v-083876c0="" class="col-md-3 col-3 mb-3 mt-3 text-center">
                                        <router-link :to="{name:'invite'}" class="">
                                            <i class="fa-solid fa-user-plus fa-fw" style="font-size: 25px;color: #C89831;"></i> <br>
                                            <!-- <img data-v-083876c0="" :src="$asseturl+'frontend/img/invite.jpeg'" width="40px" /> <br data-v-083876c0="" /> -->
                                            <span data-v-083876c0="" class="font-17">Invite Friend</span>
                                        </router-link>
                                    </div>
                                    <div data-v-083876c0="" class="col-md-3 col-3 mb-3 mt-3 text-center">
                                        <router-link :to="{name:'Withdraw'}" class="">
                                            <i class="fa-solid fa-money-bill-transfer fa-fw" style="font-size: 25px;color: #C89831;"></i> <br>
                                            <!-- <img data-v-083876c0="" :src="$asseturl+'frontend/img/withdraw.jpeg'" width="40px" /> <br data-v-083876c0="" /> -->
                                            <span data-v-083876c0="" class="font-17">Cashout</span>
                                        </router-link>
                                    </div>

                                    <div data-v-083876c0="" class="col-md-3 col-3 mb-3 mt-3 text-center">
                                        <router-link :to="{name:'companyPro'}" class="">
                                            <i class="fa-solid fa-circle-info fa-fw" style="font-size: 25px;color: #C89831;"></i> <br/>
                                            <!-- <img data-v-083876c0="" :src="$asseturl+'frontend/img/company.jpeg'" width="40px" /> <br data-v-083876c0="" /> -->
                                            <span data-v-083876c0="" class="font-17">Company Info</span>
                                        </router-link>
                                    </div>

                                </div>
                            </div>
                            <blockquote data-v-083876c0="" class="blockquote mt-4">
                                <footer data-v-083876c0="" class="blockquote-footer">
                                    Plan Limit:
                                </footer>
                            </blockquote>
                            <div data-v-083876c0="" class="row container2">
                                <div data-v-083876c0="" class="col-md-6" v-for="(plan,index) in row" :key="index">
                                    <router-link :to="{name:'Recharge'}" class="">
                                        <div data-v-083876c0="" class="card text-left plan-card">
                                            <span data-v-083876c0="" class="plan-title">{{ plan.name }}</span> <span data-v-083876c0="" class="plan-price"><i data-v-083876c0="" class="fa-solid fa-sack-dollar mr-2"></i> {{ parseFloat(plan.comission_rate*plan.totalorder).toFixed(2) }} %  ({{ plan.start_balance }}TK-{{ plan.end_balance }}TK)</span>
                                        </div>
                                    </router-link>
                                </div>
                            </div>




                        </div>






















    <div class="notice" v-if="notice">

<p> <h3>Welcome</h3>
    <br>
    <p v-html="settings.notice"></p>
</p>

        <button class="closebtn" @click="closeNotice">Close</button>
    </div>




  </div>
</template>

<script>


export default {
    created() {
        if(localStorage.getItem('notice')==1){
             this.notice = false
        }
        this.getplans()
        // console.log(this.slider.length)
        var indexx = 1;
        this.slideimage = this.slider[0]
        setInterval(() => {
            if(indexx===this.slider.length)indexx = 0;
            // console.log(indexx);
            this.slideimage = this.slider[indexx]
            indexx++
        }, 5000);

    },
    data(){
        return {
            row:{},
            notice:true,
            slideimage:'',
            settings:{},
            teamdetails:{},
            user:{
                user:{},
                plans:{}
            },
            slider:[
            this.$asseturl+'frontend/img/Daily-Task-Commisson-Chart.png',
            this.$asseturl+'frontend/img/Refar-Deposit-Bonus.png',
            this.$asseturl+'frontend/img/Refar-Task-income.png',
            this.$asseturl+'frontend/img/Self-Deposit.png'
            ]
        }
    },
    methods: {
        generator(){
        return '#'+(Math.random()*0xFFFFFF<<0).toString(16);
      },

        closeNotice(){
            this.notice = false

        localStorage.setItem('notice',1)

        },

       async getplans(){

        var resN = await this.callApi('get',`/api/admin/setting`,[])
              this.settings = resN.data


            var res = await this.callApi('get',`/api/admin/plan`,[])
            this.row = res.data


             var id = localStorage.getItem('userid');
            var results = await this.callApi('get', `/api/admin/user/${id}`, []);
            this.user = results.data;

            var teamdetailsres = await this.callApi('get',`/api/front/teamdetails?id=${id}`,[]);
            this.teamdetails = teamdetailsres.data;


        }
    },

}
</script>
<style>

section#topbar .title {
    display: flex !important;
    justify-content: space-between;
    padding: 0 18px;
}

#marquee {
  margin: 24px 0;
}

    .reImage {
        padding: 6px;
background: #e1e1e1;
box-shadow: 0px 0px 10px 0px #0000008c;
    }
    .notice {
    width: 340px;
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
    background: #f0f0f0;
    padding: 52px 11px;
    z-index: 99;
    border-radius: 10px;
    text-align: center;
}

.closebtn {
  position: absolute;
  bottom: 7px;
  background: transparent;
  border: 2px solid #7683aa;
  border-radius: 6px;
  right: 23px;
}
.plansdesign{
    display: flex;
justify-content: space-between;
}

/* .planPack {
  width: 30%;
} */

</style>
