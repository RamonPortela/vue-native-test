<template>
    <Page>
        <ActionBar title="Welcome to NativeScript-Vue!"/>
        <ListView class="list-group" for="wifi in wifiApList" @itemTap="onItemTap" style="height:100%">
            <v-template>
                <FlexboxLayout flexDirection="row" class="list-group-item">
                    <Label :text="wifi" class="list-group-item-heading"
                        style="width: 100%;" />
                </FlexboxLayout>
            </v-template>
        </ListView>
    </Page>
</template>

<script>
  import * as application from "tns-core-modules/application";

  export default {
    data() {
      return {
        msg: 'Hello World! alterado!',
        batteryLevel: '',
        wifiApList: [],
      }
    },

    methods: {
        getWifiList(){
            const context = android.content.Context;
            const wifi_service = application.android.context.getSystemService(context.WIFI_SERVICE);
            wifi_service.setWifiEnabled(true);  

            const rs = wifi_service.startScan();

            application.android.registerBroadcastReceiver(
            android.net.wifi.WifiManager.SCAN_RESULTS_AVAILABLE_ACTION
            , (context, intent) => {    
                const aps = wifi_service.getScanResults();
                for(let i = 0; i < aps.size(); i++){
                    this.wifiApList.push(aps.get(i).SSID.toString());
                }

                this.wifiApList.sort();
            });
        },

        onItemTap(args){
            console.log('item tapped: ' + args.item);
        }
    },

    created(){
        this.getWifiList();
    }
  }
</script>

<style scoped>
    ActionBar {
        background-color: #53ba82;
        color: #ffffff;
    }

    Label{
        color: #333333;
    }
</style>
