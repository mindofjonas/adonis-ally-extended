
## Registering provider

The provider will be registered inside `start/app.js` file.

```js
const providers = [
  '@mikield/adonis-ally-extended/ServiceProvider'
]
```

Add additional fields to `config/services.js` file.

```js
...
/*
 |--------------------------------------------------------------------------
 | Vk Configuration
 |--------------------------------------------------------------------------
 |
 | You can access your application credentials from the vk developers
 | page. https://vk.com/apps?act=manage
 |
 */
vk: {
  clientId: Env.get('VK_CLIENT_ID'),
  clientSecret: Env.get('VK_CLIENT_SECRET'),
  redirectUri: `${Env.get('APP_URL')}/authenticated/vk`
},

/*
 |--------------------------------------------------------------------------
 | Twitch Configuration
 |--------------------------------------------------------------------------
 |
 | You can access your application credentials from the twitch developers
 | dashboard. https://dev.twitch.tv/dashboard
 |
 */
twitch: {
  clientId: Env.get('TWITCH_CLIENT_ID'),
  clientSecret: Env.get('TWITCH_CLIENT_SECRET'),
  redirectUri: `${Env.get('APP_URL')}/authenticated/twitch`
},

/*
 |--------------------------------------------------------------------------
 | Mixer Configuration
 |--------------------------------------------------------------------------
 |
 | You can access your application credentials from the Mixer developers
 | lab. https://mixer.com/lab/oauth
 |
 */
mixer: {
  clientId: Env.get('MIXER_CLIENT_ID'),
  clientSecret: Env.get('MIXER_CLIENT_SECRET'),
  redirectUri: `${Env.get('APP_URL')}/authenticated/mixer`
},

/*
 |--------------------------------------------------------------------------
 | Patreon Configuration
 |--------------------------------------------------------------------------
 |
 | You can access your application credentials from the Patreon developers
 | portal. https://www.patreon.com/portal
 |
 */
patreon: {
  clientId: Env.get('PATREON_CLIENT_ID'),
  clientSecret: Env.get('PATREON_CLIENT_SECRET'),
  redirectUri: `${Env.get('APP_URL')}/authenticated/patreon`
}
...
```

By default all drivers are not registered, as many of them you will not use, you shall `use` needed to your project

Open `start/hooks.js` and `use` the drivers by names.
#### Registering the driver should be in `providersRegistered` hook.

Awaiable drivers are:
* twitch //twtich.tv
* mixer //mixer.com
* vk //vk.com
* patreon //patreon.com

#### Example:
```js
hooks.after.providersRegistered(() => {
  const AllyExtended = use('@mikield/ally-extended')
  AllyExtended.use('twitch').use('mixer').use('vk').use('patreon')
})
```


## Usage

Now you can access, the `ally` object on each HTTP request

```js
Route.get('/:service', async ({ request, ally }) => {
  let {service} = await request.all()
  await ally.driver(service).redirect()
})

Route.get('authenticated/:service', async ({ request, ally }) => {
  let {service} = await request.all()
  const user = await ally.driver(service).getUser()

  return user
})
```
