<template>
  <div class="login">
    <div id="okta-signin-container"></div>
  </div>
</template>

<script>
import OktaSignIn from '@okta/okta-signin-widget'
import '@okta/okta-signin-widget/dist/css/okta-sign-in.min.css'

export default {
  name: 'Login',
  mounted: function () {
    this.$nextTick(function () {
      this.widget = new OktaSignIn({
        baseUrl: 'https://thefamousfive.okta.com',
        clientId: '0oa22l2g1MY3oxjHt5d6',
        redirectUri: 'http://localhost:8080/login/callback',
        authParams: {
          pkce: true,
          issuer: 'https://thefamousfive.okta.com/oauth2/default',
          display: 'page',
          scopes: ['openid', 'profile', 'email']
        }
      })
      this.widget.renderEl(
        { el: '#okta-signin-container' },
        () => {
          /**
           * In this flow, the success handler will not be called because
           * there's a redirect to the Okta org for the authentication workflow.
           */
        },
        (err) => {
          throw err
        }
      )
    })
  },
  destroyed () {
    // Remove the widget from the DOM on path change
    this.widget.remove()
  }
}
</script>