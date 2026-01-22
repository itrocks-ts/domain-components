[![npm version](https://img.shields.io/npm/v/@itrocks/domain-components?logo=npm)](https://www.npmjs.org/package/@itrocks/domain-components)
[![npm downloads](https://img.shields.io/npm/dm/@itrocks/domain-components)](https://www.npmjs.org/package/@itrocks/domain-components)
[![GitHub](https://img.shields.io/github/last-commit/itrocks-ts/domain-components?color=2dba4e&label=commit&logo=github)](https://github.com/itrocks-ts/domain-components)
[![issues](https://img.shields.io/github/issues/itrocks-ts/domain-components)](https://github.com/itrocks-ts/domain-components/issues)
[![discord](https://img.shields.io/discord/1314141024020467782?color=7289da&label=discord&logo=discord&logoColor=white)](https://25.re/ditr)

# domain-components

Reusable domain mixins to enrich entities with common attributes (currently: active, name).

## Installation

```bash
npm i @itrocks/domain-components
```

## Usage examples

### Option A: Explicit in code

Use [@Use](https://github.com/itrocks-ts/use) when you want the composition to be explicit in source code.

```ts
import { Active }  from '@itrocks/domain-components'
import { HasName } from '@itrocks/domain-components'
import { Use }     from '@itrocks/use'

@Use(Active)
export class Project
{
	// Project now has active: boolean
}

@Use(HasName)
export class Category
{
	// Category now has name: string
}

@Use(Active, HasName)
export class User
{
	// User now has both active: boolean, and name: string
}
```

### Option B: By project configuration

Use [compose()](https://github.com/itrocks-ts/compose)
when you want to decide compositions at application level or per deployment,
without touching the library’s source code.

Call `compose()` **before** loading the modules you want to affect.

```ts
// main.ts
import { compose } from '@itrocks/compose'

compose(__dirname, {
	// add mixins to the exported User class of @itrocks/user
	'@itrocks/user': [
		'@itrocks/domain-components:Active',
		'@itrocks/domain-components:HasName'
	]
})

// only import your app after compose() is called
await import('./app.js')
```

## Component reference

### Active

Adds an `active` flag to your entity.

**Properties:**
- `active: boolean` (default: `false`, [@Required()](https://github.com/itrocks-ts/required))

**Example:**
```ts
import { Active } from '@itrocks/domain-components'
import { Use }    from '@itrocks/use'

@Use(Active)
export class Product
{
}

const product = new Product()
product.active = true
```

### HasName

Adds a `name` field to your entity.

**Properties:**
- `name: string` (default: `''`, [@Required()](https://github.com/itrocks-ts/required))

**Example:**
```ts
import { HasName } from '@itrocks/domain-components'
import { Use }     from '@itrocks/use'

@Use(HasName)
export class Team
{
}

const team = new Team()
team.name = 'Red team'
```
