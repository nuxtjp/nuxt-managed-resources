# @nuxtjp/managed-resources

A provider-neutral Nuxt module for presenting caller-owned managed resources,
repository readiness, accountability, and resource linkages.

The package only validates, filters, summarizes, and renders descriptors. It
does not discover resources, read a filesystem, fetch a provider, authorize a
user, or execute actions.

## Install

Use a versioned package artifact so the consumer remains independent:

```bash
pnpm add ./vendor/nuxtjp-managed-resources-0.1.0.tgz
```

```ts
export default defineNuxtConfig({
  modules: ['@nuxtjp/managed-resources']
})
```

```vue
<NuxtJpManagedResources
  :resources="document.resources"
  detail-base-path="/resources"
/>
```

The module auto-imports:

- `NuxtJpManagedResources`
- `NuxtJpManagedResourceCard`
- `NuxtJpStatusBadge`
- `NuxtJpConnectionList`
- `useManagedResources`
- `isManagedResourcesDocument`
- `isManagedResourceDetailDocument`

## Contract

The closed list and detail contracts use:

- `nuxtjp://managed-resources/list/v1`
- `nuxtjp://managed-resources/detail/v1`

Unknown fields and `external_actions: true` are rejected. The list guard also
requires `resource_count === resources.length`.

Repository identity keeps three concerns separate:

- `organization_id`: the organization holding operating custody;
- `placement_scope`: the classified local placement;
- `source_organization_id`: an optional, separately registered source host.

`source_organization_id: null` means that no distinct source host is declared;
it must not be inferred from a local path.

```ts
import { isManagedResourcesDocument } from '@nuxtjp/managed-resources/guards'

function acceptProjection(value: unknown) {
  if (!isManagedResourcesDocument(value)) {
    throw new Error('Invalid managed-resources projection')
  }
  return value
}
```

Types are exported from `@nuxtjp/managed-resources/types`, pure selectors from
`@nuxtjp/managed-resources/core`, and the JSON Schema from
`@nuxtjp/managed-resources/schema`.

The consumer owns acquisition, authentication, authorization, storage, and
detail-page routing. The module owns only its strict display contract and
accessible presentation.

## Development

```bash
pnpm install --offline --frozen-lockfile
pnpm typecheck
pnpm test
pnpm build
pnpm pack
```

Tests validate one fixture through both the published JSON Schema and runtime
guards, including unknown-field and cross-field failures.

## Repository boundary

This is an independent NuxtJP package. Consumers use a pinned package artifact,
not a cross-repository source path. Package and schema migrations are explicit;
the module does not provide silent compatibility aliases.

NuxtJP is an independent Japanese Nuxt community and is not represented as an
upstream-certified official localization.
