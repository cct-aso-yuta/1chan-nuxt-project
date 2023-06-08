<template>
  <header>
    <h1>1chan</h1>
    <div>
      <button type="button" @click="router.push('/login')">Log In</button>
    </div>
  </header>
</template>

<style>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
header button {
  padding: 8px;
  color: white;
  background-color: blue;
  cursor: pointer;
}
</style>

<script setup>
const router = useRouter();
</script>
