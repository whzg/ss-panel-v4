<template>

    <div class="content-padder content-background">
        <div class="uk-section-small uk-section-default header">
            <div class="uk-container uk-container-large">
                <h3><span class="ion-speedometer"></span> {{ $t("user-nav.dashboard") }} </h3>
            </div>
        </div>
        <div class="uk-section-small">
            <div class="uk-container uk-container-large">
                <div uk-grid class="uk-child-width-1-1@s uk-child-width-1-4@m">
                    <div>
                        <div class="uk-card uk-card-default uk-card-body">
                            <span class="statistics-text">{{$t("nav.download")}}</span><br/>
                            <div>{{$t("user-index.over-gfw")}}：<a href="/client/Shadowsocks-4.1.1.zip">Windows</a>/<a href="/client/shadowsocks--universal-4.6.1.apk">Android</a>/<a href="/client/ShadowsocksX-NG.1.7.1.zip">Mac</a></div>
                            <div><a href="/client/SSTap-beta-setup-1.1.0.1.exe">{{$t("user-index.game-accelerator")}}</a></div>
                            <div><a href="/document">{{$t("user-nav.document")}}</a></div>
                        </div>
                    </div>
                    <div>
                        <div class="uk-card uk-card-default uk-card-body">
                            <span class="statistics-text">{{$t("ss.traffic_total")}}</span><br/>
                            <span class="statistics-number">
                                    {{$store.state.user.traffic.total}}
                            </span>
                        </div>
                    </div>
                    <div>
                        <div class="uk-card uk-card-default uk-card-body">
                            <span class="statistics-text">{{$t("ss.traffic_used")}}</span><br/>
                            <span class="statistics-number">
                                   {{$store.state.user.traffic.used}}
                            </span>
                        </div>
                    </div>
                    <div>
                        <div class="uk-card uk-card-default uk-card-body">
                            <span class="statistics-text">{{$t("ss.traffic_unused")}}</span><br/>
                            <span class="statistics-number">
                                    {{$store.state.user.traffic.unused}}
                            </span>
                        </div>
                    </div>
                </div>
                <div uk-grid class="uk-child-width-1-1@s uk-child-width-1-2@l">
                    <div>
                        <div class="uk-card uk-card-default">
                            <div class="uk-card-header">
                                <span class="uk-margin-small-right" uk-icon="icon: check"></span>
                                {{$t("user-index.checkin")}}
                            </div>
                            <div class="uk-card-body">
                                <p>
                                    {{$t("user-index.last-checkin-at")}}: 
                                    <em>{{$store.state.user.checkIn.lastCheckInTime}}</em>
                                    <button v-if="$store.state.user.checkIn.canCheckIn" class="uk-margin-left uk-button uk-button-primary" @click="checkIn">
                                        {{$t("user-index.checkin")}}
                                    </button>
                                </p>
                            </div>
                        </div>
                        <div class="uk-card uk-card-default uk-margin-top">
                            <div class="uk-card-header">
                                <span class="uk-margin-small-right" uk-icon="icon: cart"></span>
                                {{$t("user-index.recharge")}}
                            </div>
                            <div class="uk-card-body">
                                <p>
                                    {{$t("user-info.expire-time")}}: 
                                    <em>{{timeFormat($store.state.user.data.expire_time)}}</em>
                                    <router-link :to="{ name: 'recharge'}" exact>
                                        <button class="uk-margin-left uk-button uk-button-primary">
                                            {{$t("user-index.recharge")}}
                                        </button>
                                    </router-link>
                                    <!--支付宝充值功能暂时出问题了，下面是临时的-->
                                    <!-- <a href="/">
                                        <button class="uk-margin-left uk-button uk-button-primary">{{$t("user-index.wechat-recharge")}}</button>
                                    </a> -->
                                </p>
                            </div>
                        </div>
                    </div>
                    <div>
                        <div class="uk-card uk-card-default">
                            <div class="uk-card-header">
                                <span class="uk-margin-small-right" uk-icon="icon: shrink"></span>
                                {{$t("user-index.connection-info")}}
                            </div>
                            <div class="uk-card-body">
                                <p>{{$t("ss.port")}}: <em>{{$store.state.user.data.port}}</em></p>
                                <p>{{$t("ss.password")}}: <em>{{$store.state.user.data.passwd}}</em></p>
                                <p>{{$t("ss.method")}}: <em>{{$store.state.user.data.method}}</em></p>
                                <p>{{$t("ss.obfs-protocol")}}: <em>{{$store.state.user.data.protocol}}</em></p>
                                <p>{{$t("ss.obfs-plugin")}}: <em>{{$store.state.user.data.obfs}}</em></p>
                                <p>{{$t("ss.obfs_param")}}: <em>{{$store.state.user.data.obfs_param}}</em></p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

</template>

<script>
    import rest from '../http/rest'
    export default {
        name: 'Dashboard',
        components: {},
        data () {
            return {
                data: []
            }
        },
        methods: {
            checkIn() {
                rest.post('checkIn')
                    .then(response => {
                        let traffic = response.data.traffic;

                        UIkit.notification({
                            message: this.$t('user-index.traffic-got') + traffic,
                            status: 'primary',
                            pos: 'top-center',
                            timeout: 5000
                        });

                    })
                    .catch(e => {
                    });

                console.log("check in");
                this.$store.state.user.checkIn.canCheckIn = false;
            },
            timeFormat(ut){
                return new Date(ut * 1e3).toLocaleDateString();
            },
        },
        mounted: function () {

        },
    }
</script>