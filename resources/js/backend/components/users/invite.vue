<template>
    <div>



        <section id="bigbg" style="background: #3838a3 !important;height: 100vh;margin-top: 41px;" class="qr">
            <div class="container-fluid">
                <a href="javascript:void(0)" @click="$router.go(-1)"><i class="fa fa-angle-left"></i></a>
                <div class="QR  text-center">
                    <!-- <img src="img/QR.png"> -->
                    <img :src="'https://chart.googleapis.com/chart?chs=200x200&cht=qr&chl='+ref+'&choe=UTF-8'" :alt='ref'/>
                </div>
                <form action="" class="link" style="width: 70%;margin: 10px auto;">
                    <div class="row">
                        <input class="col-10" style="color: white;padding: 10px 8px;" disabled type="url" v-model="ref">
                        <button style="color: black;padding: 10px 8px;" type="button" class="copyBtn copy col-2" :data-clipboard-text="ref">copy</button>

                        <!-- <input class="copy col-2" style="color: black;padding: 10px 8px;" type="button" value="copy" @click="copyref"> -->
                    </div>
                </form>


                <div class="copyPopup" v-if="popup">
                    <span> Refer link copied success</span>
                </div>
                <!-- <div class="download-link">
                    <a href="download.html" class="download"><i class="fa fa-shopping-bag"></i> Download address </a>
                </div> -->
            </div>
        </section>


    </div>
</template>

<script>
import ClipboardJS from 'clipboard';
export default {
    created() {

        const clipboard = new ClipboardJS('.copyBtn');
        clipboard.on('success', (e)=> {
            this.popup = true
                setTimeout(() => {
                    this.popup = false
                }, 1000);
        });
        clipboard.on('error', (e)=> {
            // this.notifiy('Failed to copy to clipboard!');
        });



        this.getData()
    },
    data() {
        return {
            ref: '',
            row:{},
            popup:false
        }
    },
    methods: {

          async getData() {
             var id = localStorage.getItem('userid');
            var res = await this.callApi('get', `/api/admin/user/${id}`, []);
                this.ref = window.location.origin+'/register?ref='+res.data.user.username
            this.row = res.data;

        },


        copyref() {


            navigator.clipboard.writeText(this.ref);
          this.popup = true

            setTimeout(() => {
                this.popup = false
            }, 1000);
        }
    },
}
</script>
<style>

.copyPopup {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%);
  background: #000000b5;
  padding: 27px;
}
.copyPopup span {
  display: block;
  font-size: 25px;
  color: white;
}
</style>
