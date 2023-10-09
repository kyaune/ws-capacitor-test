<script setup>
import {onBeforeUnmount, onMounted} from "vue";
import { Network } from '@capacitor/network';

const ws = new WebSocket('ws://89.17.55.74:55558/')
ws.onopen = (e) => {
  console.log('open!', e)
}

ws.onmessage = (e) => {
  console.log('message!', e)
}

ws.onclose = (e) => {
  console.log('closed!', e)
}

ws.onerror = (e) => {
  console.log('error!', e)
}

Network.addListener('networkStatusChange', status => {
  console.log('Network status changed', status);
});

const logCurrentNetworkStatus = async () => {
  const status = await Network.getStatus();

  console.log('Network status:', status);
};

const closeWs = () => {
  ws.close()
}

const getToken = () => {
  let message = {
    "command": 'get_token',
    "username": 'mimismart',
    "password": "1212121212121212"
  }
  message = JSON.stringify(message)
  ws.send(message)
}

onMounted(() => {
  console.log('mounted')
})

onBeforeUnmount(() => {
  closeWs()
})

</script>

<template>
  <div class="container">
    <div>
    Hello!!!
    <button @click="getToken">Token</button>
    <button @click="closeWs">Close</button>
    <button @click="logCurrentNetworkStatus">Network status</button>
    </div>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
