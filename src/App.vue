<script setup>
import { ref } from 'vue'
import Loader from './components/Loader.component.vue'

try {
  if (window.location.pathname.split('/')[1].length > 0) {
    fetch(`${import.meta.env.VITE_API_URL}/${window.location.pathname.split('/')[1]}`, {
      method: 'GET',
      headers: {
        'Content-Type': 'application/json'
      }
    })
      .then(res => res.json())
      .then(data => {
        if (data.ok) {
          window.location.href = data.data[0].originUrl
        } else throw new Error('error redirecting the original request')
      })
  }
} catch (error) { console.error(error) }

</script>

<template>
  <Loader />
</template>

<style scoped></style>
