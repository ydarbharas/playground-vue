<template>
  <v-app>
    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>Sarah</span>
        <span class="font-weight-light">Brady</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>
 

    <v-content>
      <Home/>
      <!-- <Form/> -->
      <CreateProject/>
    </v-content>
  </v-app>
</template>

<script>
import Home from './views/Home'
import Form from './components/Form'
import CreateProject from './components/CreateProject'

export default {
  name: 'App',
  components: { Home, Form, CreateProject },
  data () {
    return {
      //
    }
  }
}
</script>
