# cheauth-nuxt

Dependencies: 
1.  <a href="https://github.com/nuxt-community/auth-module">
        https://github.com/nuxt-community/auth-module
    </a>
    (npm install @nuxtjs/auth-next @nuxtjs/axios),
    <a href="https://axios.nuxtjs.org">
2.  https://axios.nuxtjs.org
    </a>
    (npm install @nuxtjs/axios),
    <a href="https://bootstrap-vue.org">
3.  https://bootstrap-vue.org
    </a>
    (npm install bootstrap-vue)


Setting up:
1.  Add in nuxt.config.js:
        <code>
        const cheauthUrl = process.env.NODE_ENV === 'development' ? "http://127.0.0.1:5017/cheauth/" : "http://109.234.37.52/cheauth/"
        env: {
            cheauthUrl: cheauthUrl
        },
        auth: {
            strategies: {
                <NAME>: {
                    scheme: 'refresh',
                    token: {
                        property: 'access',
                        maxAge: 1800,
                        // type: 'Bearer'
                    },
                    refreshToken: {
                        property: 'refresh',
                        data: 'refresh',
                        maxAge: 60 * 60 * 24 * 30
                    },
                    user: {
                        property: 'user',
                        autoFetch: true
                    },
                    endpoints: {
                        login: { url: cheauthUrl + 'authorization/', method: 'post' },
                        refresh: { url: cheauthUrl + 'authorization/token/refresh/', method: 'post' },
                        user: { url: cheauthUrl + 'authorization/user-data/', method: 'get' },
                        logout: { url: cheauthUrl + 'authorization/logout/', method: 'post' }
                    },
                    autoLogout: true,
                    redirect: false,
                }
            }
        },
        </code>
2.  Import in component:
        <code>
        import Mregistration from "@/components/main/cheauth-nuxt/registration.vue";
        import Mauth from "@/components/main/cheauth-nuxt/auth.vue";
        </code>
3.  Call modals with "v-b-modal.modal-au" and "v-b-modal.modal-reg"
