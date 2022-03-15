# resolve-bit-path

Resolves a file within a BIT archive based on how web browsers would load it.

```js
const resolveBitPath = require('@web4/resolve-bit-path')

const archive = getABitdrivedriveSomehow();

const rawPath = '/blog/about'

try {
  const {type, path, stat} = await resolveBitPath(archive, rawPath)
  if(type === 'directory') {
    console.log('Render the file list from the folder signified by `path`')
  } else if(type === 'file') {
    console.log('Render the file at `path`')
  } else {
    console.error('Something went horribly wrong')
  }
} catch (err) {
  console.log('Show your application 404 page')
}
```

## How it works

How the algorithm for looking up paths works:

1. It will look for the `web_root` property in the `/bit.json` file to use as prefix, if non-existent it will use `/`.
2. It will look for a file to be returned, with following order at:

    1. exactly the path
    2. with an `.html` suffix
    3. with an `.md` suffix
    4. with an `/index.html` suffix
    5. with an `/index.md` suffix

3. It will look for a folder to be returned at the given path.
4. It will look for the `fallback_page` property in the `/bit.json`, will return the file for the path:

    1. if it exists as is
    2. if it exists with the `web_root` prefix

5. It will throw an `Not Found` error.

## License

[MIT](./LICENSE)
