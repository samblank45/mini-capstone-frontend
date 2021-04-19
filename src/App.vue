<template>

  <div id="app">
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
      <router-link class="navbar-brand" to="/">Home</router-link>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent"                 aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>

    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav mr-auto">
        <li class="nav-item active">
          <router-link class="nav-link" to="/login">Login </router-link>
        </li>
        <li class="nav-item active">
          <router-link class="nav-link" to="/signup">Signup </router-link>
        </li>
        <li class="nav-item active">
          <router-link class="nav-link" to="/logout">Logout </router-link>
        </li>
        <li class="nav-item active">
          <router-link class="nav-link" to="/products/new">New Post </router-link>
        </li>
        <li class="nav-item active">
          <router-link class="nav-link" to="/logout">Logout </router-link>
        </li>
      </ul>
    </div>
  </nav>

    <!-- <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div> -->
    <router-view/>
  </div>
</template>

<style>
</style>
