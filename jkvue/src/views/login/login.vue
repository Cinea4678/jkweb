<template>
    <div class="login_board">
        <video class="video-background" preload="auto" loop playsinline autoplay src="../../video/loginbck.mp4"
            muted="muted"></video>
        <button id="login_ball" v-on:click="show_board">
            <span class="login_title" v-show="show_login">Login</span>
        </button>
        <button id="ball_1" v-show="show_ball">
        </button>
        <button id="ball_2" v-show="show_ball" v-on:click="login">
            <!--这个同时也是login按钮-->
            <span class="ball_2_title" v-show="show_ball_2">Sign In</span>
        </button>
        <button id="ball_3" v-show="show_ball" v-on:click="ToRegister">
            <span class="ball_3_title" v-show="show_ball_3">Go To Sign Up</span>
            <!--这个同时也是sign up按钮-->
        </button>
        <form class="login_form" v-show="show_b">
            <p class="Tishi" v-show="showTishi">{{ tishi }}</p>
            <input class="input" type="text" id="username" placeholder="username" v-model="username">
            <input class="input" type="password" id="password" placeholder="password" v-model="password">
        </form>
        <!--模拟各星球(各星球为对应的tags)围绕login公转-->
        <div class='orbit1'>
            <div class='planet1'>
                <p style="transform:rotate(-60deg)">science</p>
            </div>
        </div>

        <div class='orbit2'>
            <div class='planet2'>
                <p style="transform:rotate(-30deg)">study</p>
            </div>
        </div>

        <div class='orbit3'>
            <div class='planet3'>
                <p style="transform:rotate(-60deg)">work</p>
            </div>
        </div>

        <div class='orbit4'>
            <div class='planet4'>
                <p style="transform:rotate(-60deg)">arts</p>
            </div>
        </div>


        <div class='orbit5'>
            <div class='planet5'>
                <p style="transform:rotate(-60deg)">news</p>
            </div>
        </div>

        <div class='orbit6'>
            <div class='planet6'>
                <p style="transform:rotate(-60deg)">math</p>
            </div>
        </div>

        <div class='orbit7'>
            <div class='planet7'>
                <p style="transform:rotate(-80deg)">phsics</p>
            </div>
        </div>

        <div class='orbit8'>
            <div class='planet8'>
                <p style="transform:rotate(-80deg)">cpu</p>
            </div>
        </div>
        <!--下面为页面右侧的站名展示-->
        <div
            style="animation:J 6s infinite; font: italic 2em Georgia, serif; color:aliceblue ;font-size: 50px;position:absolute; right:100px;">
            J</div>
        <div
            style="animation:K 6s infinite; font: italic 2em Georgia, serif; color:aliceblue ;font-size: 50px;position:absolute; right:100px;">
            K</div>
        <div
            style="animation:W 6s infinite; font: italic 2em Georgia, serif; color:aliceblue ;font-size: 50px;position:absolute; right:100px;">
            W</div>
        <div
            style="animation:E 6s infinite;  font: italic 2em Georgia, serif;color:aliceblue; font-size: 50px;position:absolute; right:100px;">
            E</div>
        <div
            style="animation:B 6s infinite; font: italic 2em Georgia, serif; color:aliceblue; font-size: 50px;position:absolute; right:100px;">
            B</div>
        <div class="image">
            <img src="../../picture/ufo.png" alt="UFO" width="80" height="80">
        </div>
    </div>
</template>

<script>
import { setCookie, getCookie } from '../../assets/js/cookie.js'
export default {
    data() {
        return {
            username: "",
            password: "",
            showTishi: false,
            show_b: false,
            tishi: "这里是提示",
            canclick: true,
            show_login: true,
            show_ball: false,
            show_ball_2: false,
            show_ball_3: false
        }
    },
    methods: {
        login() {
            if (this.username == "" || this.password == "") {
                this.tishi = "Please Input Your Username And Password"
                this.showTishi = true
                this.line_shake()
            } else {
                // URLSearchParams对象是为了让参数以form data形式
                let params = new URLSearchParams();
                params.append('username', this.username);
                params.append('password', this.password);
                console.log(this.username + " " + this.password);
                this.$axios.post("http://127.0.0.1:5000/user/login", params).then((res) => {
                    console.log(res.data['code'])
                    if (res.data['code'] == 405) {
                        this.tishi = "user does not exist"
                        console.log(res.data['code'])
                        this.showTishi = true
                        this.line_shake()
                    } else if (res.data['code'] == 404) {
                        this.tishi = "False Password"
                        this.showTishi = true
                        console.log(res.data['code'])
                        this.line_shake()
                    } else if (res.data['code'] == 403) {
                        this.tishi = "False Form"
                        this.showTishi = true
                        console.log(res.data['code'])
                        this.line_shake()
                    } else if (res.data['code'] == "admin") {
                        this.$router.push("/home")
                        console.log(res.data['code'])
                    } else if (res.data['code'] == 402) {
                        console.log('successful login')
                        this.tishi = "Successful Login"
                        this.showTishi = true
                        console.log(res.data['code'])
                        setCookie("username", this.username, 1000 * 60)
                        setTimeout(function () {
                            this.$router.push({ name: "home", params: { "uid": res.data['id'] } });
                        }.bind(this), 1000)
                    }
                })
            }
        },
        ToRegister() {
            this.$router.push("/register")
        },
        show_board() {
            //分3段动画，第一个圈原地缩小，第二个到第四个圈弹跳出现，展开成登录界面
            var n1 = 0;//第一区间段的计时器
            var n2 = 0;//第二区间段的计时器
            var n3 = 0;//第三区间段的计时器
            var n4 = 0;//第四区间段的计时器
            if (this.canclick) {
                var timer = setInterval(() => {
                    this.show_login = false;
                    if (n1 <= 25) {
                        login_ball.style.marginTop = 150 + 'px';//由于更改了login图标的大小和初始位置，在这里也要改
                        login_ball.style.marginLeft = 550 + 'px';
                        login_ball.style.height = (300 - n1 * 10) + 'px';
                        login_ball.style.width = (300 - n1 * 10) + 'px';
                        login_ball.style.radius = (300 - n1 * 10) + 'px';
                        n1++;
                    } else if (n2 <= 20) {
                        this.show_ball = true;
                        ball_2.style.top = 6 * n2 + 'px';
                        n2++;
                    } else if (n2 <= 40) {
                        ball_3.style.top = 6 * n2 + 'px';
                        n2++;
                    } else if (n3 <= 40) {
                        this.show_b = true;
                        ball_2.style.width = (50 + 4 * n3) + 'px';
                        n3++;
                    } else if (n4 <= 40) {
                        this.show_ball_2 = true;
                        ball_3.style.width = (50 + 4 * n4) + 'px';
                        n4++;
                    } else {
                        this.show_ball_3 = true;
                        clearInterval(timer);
                    }
                }, 10);
            }
            this.canclick = false;
        },
        line_shake() {
            var n = 0;
            var timer = setInterval(() => {
                if (n <= 10) {
                    login_ball.style.marginLeft = 550 + 5 * n + 'px';
                    ball_1.style.marginLeft = 550 + 5 * n + 'px';
                    ball_2.style.marginLeft = 550 + 5 * n + 'px';
                    ball_3.style.marginLeft = 550 + 5 * n + 'px';
                    username.style.marginLeft = 620 + 5 * n + 'px';
                    password.style.marginLeft = 620 + 5 * n + 'px';
                    n++;
                } else if (n <= 20) {
                    login_ball.style.marginLeft = 650 - 5 * n + 'px';
                    ball_1.style.marginLeft = 650 - 5 * n + 'px';
                    ball_2.style.marginLeft = 650 - 5 * n + 'px';
                    ball_3.style.marginLeft = 650 - 5 * n + 'px';
                    username.style.marginLeft = 720 - 5 * n + 'px';
                    password.style.marginLeft = 720 - 5 * n + 'px';
                    n++;
                } else if (n <= 30) {
                    login_ball.style.marginLeft = 450 + 5 * n + 'px';
                    ball_1.style.marginLeft = 450 + 5 * n + 'px';
                    ball_2.style.marginLeft = 450 + 5 * n + 'px';
                    ball_3.style.marginLeft = 450 + 5 * n + 'px';
                    username.style.marginLeft = 520 + 5 * n + 'px';
                    password.style.marginLeft = 520 + 5 * n + 'px';
                    n++;
                } else if (n <= 40) {
                    login_ball.style.marginLeft = 750 - 5 * n + 'px';
                    ball_1.style.marginLeft = 750 - 5 * n + 'px';
                    ball_2.style.marginLeft = 750 - 5 * n + 'px';
                    ball_3.style.marginLeft = 750 - 5 * n + 'px';
                    username.style.marginLeft = 820 - 5 * n + 'px';
                    password.style.marginLeft = 820 - 5 * n + 'px';
                    n++;
                } else {
                    clearInterval(timer);
                }
            }, 3);
        }
    }
}
</script>

<style>
@import"login_style.css";

/*引入css文件 */
@keyframes flashing {
    0% {
        box-shadow: 0px 0px 0px cyan;
        border-color: aliceblue;
    }

    50% {
        box-shadow: 0px 0px 40px cyan;
        border-color: aliceblue;
    }

    100% {
        box-shadow: 0px 0px 0px cyan;
        border-color: aliceblue;
    }

}

#login_ball {
    z-index: 1;
    margin-top: 250px;
    height: 150px;
    width: 150px;
    margin-left: 600px;
    position: fixed;
    border-radius: 300px;
    animation: flashing 2s infinite alternate;
    color: rgb(73, 64, 64);
}

.login_form {
    z-index: 1;
    margin-top: 400px;
    position: fixed;
}

.login_title {
    z-index: 1;
    color: aqua;
    font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
    font-size: 50px;
}

#ball_1 {
    z-index: 1;
    margin-top: 270px;
    margin-left: 550px;
    height: 50px;
    width: 50px;
    border-radius: 50px;
    position: fixed;
    animation: flashing 2s infinite alternate;
}

#ball_2 {
    z-index: 1;
    margin-top: 270px;
    margin-left: 550px;
    height: 50px;
    width: 50px;
    border-radius: 50px;
    position: fixed;
    animation: flashing 2s infinite alternate;
}

#ball_3 {
    z-index: 1;
    margin-top: 270px;
    margin-left: 550px;
    height: 50px;
    width: 50px;
    border-radius: 50px;
    position: fixed;
    animation: flashing 2s infinite alternate;
}

#username {
    z-index: 1;
    margin-top: 155px;
    margin-left: 620px;
    position: fixed;
    border: none;
    border-bottom: 2px solid rgb(154, 151, 151);
    font-size: 30px;
    color: rgb(154, 151, 151);
    background-color: black;
}

#password {
    z-index: 1;
    margin-top: 275px;
    margin-left: 620px;
    border: none;
    position: fixed;
    border-bottom: 2px solid rgb(154, 151, 151);
    font-size: 30px;
    color: rgb(154, 151, 151);
    background-color: black;
}

.login_form {
    z-index: 1;
    margin-top: 0px;
    margin-left: 0px;
    height: 0;
    width: 0;
}

.Tishi {
    z-index: 1;
    margin-top: 90px;
    margin-left: 560px;
    height: 30px;
    width: 800px;
    font-size: 20px;
    color: rgb(154, 151, 151);
    position: fixed;
}

.ball_2_title {
    z-index: 1;
    font-size: 25px;
    color: cornflowerblue;
    text-align: center;
}

.ball_3_title {
    z-index: 1;
    font-size: 25px;
    color: cornflowerblue;
    text-align: center;
}

body {
    display: block;
    margin: 0px;
    margin-top: 0px;
    margin-right: 0px;
    margin-bottom: 0px;
    margin-left: 0px;
}

.video-background {
    position: fixed;
    overflow: hidden;
    background-size: cover;
}
</style>
