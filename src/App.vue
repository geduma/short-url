<script setup>
import { ref } from 'vue'
import Loader from './components/Loader.component.vue'

try {
  fetch(`${import.meta.env.VITE_API_URL}/auth`, {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    key: import.meta.env.VITE_API_KEY,
    name: 'short-url-api',
    user: 'geduramc'
  })
})
.then(res => res.json())
.then(data => {
  if (data.ok) {
    fetch(`${import.meta.env.VITE_API_URL}/${window.location.pathname.split('/')[1]}`, {
      method: 'GET',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${data.data.token}`
      }
    })
    .then(res => res.json())
    .then(data => {
      if (data.ok) {
        window.location.href = data.data[0].originUrl
      } else throw new Error('error redirecting the original request')
    })
  } else throw new Error('error resolving the url request')
})
} catch (error) console.error(error)

</script>

<template>
  <Loader />
</template>

<style scoped></style>
