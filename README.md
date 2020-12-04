# json-schema-migrate

Migrate JSON-Schema from draft-04 to draft-07 and draft-2019-09

[![build](https://github.com/ajv-validator/json-schema-migrate/workflows/build/badge.svg)](https://github.com/ajv-validator/json-schema-migrate/actions?query=workflow%3Abuild)
[![npm](https://img.shields.io/npm/v/json-schema-migrate)](https://www.npmjs.com/package/json-schema-migrate)
[![Coverage Status](https://coveralls.io/repos/github/ajv-validator/json-schema-migrate/badge.svg?branch=master)](https://coveralls.io/github/ajv-validator/json-schema-migrate?branch=master)

## Install

```
npm install json-schema-migrate@beta
```

or to install v0.2 (to migrate to draft-06)

```
npm install json-schema-migrate
```

## Usage

```javascript
const migrate = require("json-schema-migrate")
const schema = {
  id: "my-schema",
  minimum: 1,
  exclusiveMinimum: true,
}
const {valid, errors} = migrate.draft7(schema)
// or migrate.draft2019(schema)

console.log(schema)
// {
//  $id: 'my-schema',
//  exclusiveMinimum: 1
// }
```

You can access Ajv instance that was used to migrate schema using `migrate.getAjv` function:

```javascript
console.log(migrate.getAjv().errorsText(errors))
```

## Changes in schemas after migration

- `id` is replaced with `$id`
- `$schema` value becomes draft-07 or draft-2019-09 meta-schema
- boolean form of `exclusiveMaximum/Minimum` is replaced with numeric form
- `enum` with a single allowed value is replaced with `const`
- Non-standard `constant` is replaced with `const`
- empty schema is replaced with `true`
- schema `{"not":{}}` is replaced with `false`
- `draft2019` function additionally replaces:
  - `definitions` with `$defs`
  - `dependencies` with `dependentRequired` and `dependentSchemas`
  - `"id": "#foo"` with `"$anchor": "foo"`
  - `"id": "schema#foo"` with `"$id": "schema", "$anchor": "foo"`

## License

[MIT](https://github.com/epoberezkin/json-schema-migrate/blob/master/LICENSE)
