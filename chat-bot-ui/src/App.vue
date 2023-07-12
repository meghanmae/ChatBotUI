<template>
  <v-app>
    <span class="bg"></span>
    <v-app-bar :elevation="3">
      <template v-slot>
        <v-app-bar-title>
          <RouterLink to="/">
            <v-icon icon="mdi-robot-excited" color="primary" class="mb-2 mr-1" />
            Chat Bot
          </RouterLink>
        </v-app-bar-title>
        <v-spacer></v-spacer>
      </template>
    </v-app-bar>

    <v-main>
      <RouterView />
    </v-main>
  </v-app>
</template>

<script setup lang="ts"></script>
