<p>
  <img width="100%" src="https://assets.solidjs.com/banner?type=solid-supabase&background=tiles&project=%20" alt="solid-supabase">
</p>

# solid-supabase

A simple wrapper around Supabase.js to enable usage within Solid.

## Installation

```bash
npm install @supabase/supabase-js solid-supabase # or pnpm or yarn
```

For supabase-js v1, install `solid-supabase@0.2.x` instead.

## Quick start

In the root of your app, use the `SupabaseProvider` and pass the supabase client with your credentials.

```tsx
import { render } from 'solid-js/web'
import { createClient } from '@supabase/supabase-js'
import { SupabaseProvider } from 'solid-supabase'
import App from './App'

const supabase = createClient('SUPABASE_URL', 'SUPABASE_KEY')

render(
  () => (
    <SupabaseProvider client={supabase}>
      <App />
    </SupabaseProvider>
  ),
  document.getElementById('root'),
)
```

This will make the supabase client available anywhere along the component tree.

## Use the primitive

```tsx
import { Match, Switch, createResource, createSignal } from 'solid-js'
import { createSupabase } from 'solid-supabase'

const App = () => {
  const supabase = createSupabase()

  const [postId] = createSignal(1)
  const [data, { refetch }] = createResource(postId, (arg) => {
    return supabase.from('posts').select('*').eq('id', arg)
  })

  return (
    <Switch>
      <Match when={data.loading}>
        <div>Loading...</div>
      </Match>
      <Match when={data.error}>
        <div>{data.error}</div>
      </Match>
      <Match when={data()}>
        <pre>{JSON.stringify(data(), null, 2)}</pre>
        <button onClick={refetch}>Refetch</button>
      </Match>
    </Switch>
  )
}

export default App
```

Other available primitives

```ts
import {
  createOnAuthStateChange,
  createSupabaseAuth,
  createSupabaseFrom,
  createSupabaseStorage,
} from 'solid-supabase'

const auth = createSupabaseAuth() // auth.signOut()
const storage = createSupabaseStorage() // storage.listBuckets()
const from = createSupabaseFrom() // from('posts').select('*').eq('id', arg)

createOnAuthStateChange((event, session) => {
  console.log(event, session)
})
```

## License

MIT
