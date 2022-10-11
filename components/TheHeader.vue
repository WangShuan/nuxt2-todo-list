<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container-fluid">
      <NuxtLink class="navbar-brand" to="/">NUXT2 TODO LIST</NuxtLink>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarText" aria-controls="navbarText" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarText">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <NuxtLink class="nav-link" to="/">Todo list</NuxtLink>
          </li>
          <li class="nav-item">
            <NuxtLink class="nav-link" to="/about">About</NuxtLink>
          </li>
        </ul>
        <span class="navbar-text">
          Try to using Nuxt2 create todo list.
        </span>
      </div>
    </div>
  </nav>
</template>

<style lang="scss">
.header{
  background: #e0e0e0;
  padding: 15px 30px;
  & ul{
    display: flex;
    list-style: none;
    padding: 0;
    margin: 0;
    justify-content: space-between;
    align-items: center;
  }
}
</style>
