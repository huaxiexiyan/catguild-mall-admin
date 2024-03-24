<template>
  <div></div>
</template>
<script lang="ts">
export default {
  name: 'LoginIndex',
};
</script>
<script setup lang="ts">
import { useRoute, useRouter } from 'vue-router';

import { useUserStore } from '@/store';

const userStore = useUserStore();

const router = useRouter();
const route = useRoute();
const { catToken } = route.query;
if (catToken) {
  userStore.loadToken(atob(catToken.toString()));
  const redirect = route.query.redirect as string;
  const redirectUrl = redirect ? decodeURIComponent(redirect) : '/dashboard';
  router.push(redirectUrl);
} else {
  const loginURL = `http://localhost:20000/api/auth/login?redirect_uri=${encodeURIComponent(
    `http://${window.location.host}/login`,
  )}&original_domain=${encodeURIComponent(window.location.hostname)}`;
  window.location.href = loginURL;
}
</script>
