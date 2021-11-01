<template>
  <a-layout-header :style="{ position: 'fixed', zIndex: 1, width: '100%' }">
    <div class="logo" />
    <a-menu
      theme="dark"
      mode="horizontal"
      :default-selected-keys="['2']"
      :style="{ lineHeight: '64px' }"
    >
      <a-menu-item key="home">
        <NuxtLink to="/">Home</NuxtLink>
      </a-menu-item>
      <a-menu-item key="articles">
        <NuxtLink to="/articles">Articles</NuxtLink>
      </a-menu-item>
      <a-menu-item key="contact">
        <NuxtLink to="/contact-us">Contact Us</NuxtLink>
      </a-menu-item>
    </a-menu>
  </a-layout-header>
</template>
