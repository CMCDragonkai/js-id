# js-id

[![pipeline status](https://gitlab.com/MatrixAI/open-source/js-id/badges/master/pipeline.svg)](https://gitlab.com/MatrixAI/open-source/js-id/commits/master)

ID generation for JavaScript & TypeScript applications.

Example Usage:

```ts
import { IdRandom, IdDeterministic, IdSortable, utils } from './src';

// Random ids, equivalent to UUIDv4

const randGen = new IdRandom();

const randIds = [...utils.take(randGen, 3)];
console.log(randIds.map((b) => utils.toUUID(b)));

// Deterministic ids, equivalent to UUIDv5

const deteGen = new IdDeterministic({
  namespace: 'foo'
});

const deteId1 = deteGen.get();
const deteId2 = deteGen.get('bar');

console.log(utils.toUUID(deteId1));
console.log(utils.toMultibase(deteId2, 'base58btc'));

// Strictly monotonic sortable ids, equivalent to UUIDv7

let lastId = new Uint8Array(
  [
    0x06, 0x16, 0x3e, 0xf5, 0x6d, 0x8d, 0x70, 0x00,
    0x87, 0xc4, 0x65, 0xd5, 0x21, 0x9b, 0x03, 0xd4,
  ]
).buffer;

const sortGen = new IdSortable({ lastId });

const sortId1 = sortGen.get();
const sortId2 = sortGen.get();
const sortId3 = sortGen.get();

const sortIds = [
  Buffer.from(sortId2),
  Buffer.from(sortId3),
  Buffer.from(sortId1),
];

sortIds.sort(Buffer.compare);

console.log(sortIds);

// Save the last id to ensure strict monotonicity across process restarts
lastId = sortGen.lastId;
```

## Installation

```sh
npm install --save @matrixai/id
```

## Development

Run `nix-shell`, and once you're inside, you can use:

```sh
# install (or reinstall packages from package.json)
npm install
# build the dist
npm run build
# run the repl (this allows you to import from ./src)
npm run ts-node
# run the tests
npm run test
# lint the source code
npm run lint
# automatically fix the source
npm run lintfix
```

### Docs Generation

```sh
npm run docs
```

See the docs at: https://matrixai.github.io/js-id/

### Publishing

```sh
# npm login
npm version patch # major/minor/patch
npm run build
npm publish --access public
git push
git push --tags
```
