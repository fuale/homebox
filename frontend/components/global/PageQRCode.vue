<template>
  <div class="dropdown dropdown-left">
    <slot>
      <label tabindex="0" class="btn btn-neutral join-item btn-sm">
        <MdiQrcode />
      </label>
    </slot>
    <div tabindex="0" class="card compact dropdown-content z-[1] shadow-lg bg-base-100 rounded-box w-64">
      <div class="card-body">
        <h2 class="text-center">Page URL</h2>
        <img :src="getQRCodeUrl()" />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
  import { route } from "../../lib/api/base";
  import MdiQrcode from "~icons/mdi/qrcode";

  function getQRCodeUrl(): string {
    const currentURL = window.location.href;

    return route(`/qrcode`, { data: encodeURIComponent(currentURL) });
  }
</script>

<style lang="scss" scoped></style>
