<script lang="ts" setup>
import {
  ref,
  computed,
  onMounted,
  onUnmounted,
  $navigateTo,
} from 'nativescript-vue';
import { Utils, Device } from '@nativescript/core';
import Details from './Details.vue';
import * as bluetooth from 'nativescript-bluetooth';

const counter = ref(0);

function logMessage() {
  console.log('You have tapped the message!');
}

const deviceText = ref('Unknown Device');

const batteryLevel = ref('Unknown Battery Level');

if (global.isAndroid && Device.sdkVersion >= '21') {
  deviceText.value = 'Android Device';
  console.log(bluetooth);
  const batteryManager = Utils.android
    .getApplicationContext()
    .getSystemService(android.content.Context.BATTERY_SERVICE);
  batteryLevel.value = `Battery level is at ${batteryManager.getIntProperty(
    android.os.BatteryManager.BATTERY_PROPERTY_CAPACITY
  )}%`;

  const btManager = Utils.android
    .getApplicationContext()
    .getSystemService(android.content.Context.BLUETOOTH_SERVICE);
  const btAdapter = btManager.getAdapter();
  console.log(btAdapter);
}
</script>

<template>
  <Frame>
    <Page>
      <ActionBar>
        <Label text="Home" class="font-bold text-lg" />
      </ActionBar>

      <GridLayout rows="*, auto, auto, *" class="px-4">
        <Label
          row="1"
          class="text-xl align-middle text-center text-gray-500"
          :text="batteryLevel"
        />

        <Button
          row="2"
          @tap="$navigateTo(Details)"
          class="mt-4 px-4 py-2 bg-white border-2 border-blue-400 rounded-lg"
          horizontalAlignment="center"
        >
          View Details
        </Button>
      </GridLayout>
    </Page>
  </Frame>
</template>

<style>
/* .info {
    font-size: 20;
  } */
</style>
