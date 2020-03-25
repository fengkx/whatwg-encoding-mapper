# Encoding Map According to the WHATWG Encoding Standard

[![NPM](https://nodei.co/npm/whatwg-encoding-mapper.png?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/whatwg-encoding-mapper/)

```sh
npm i whatwg-encoding-mapper
```

This package is a fork of [whatwg-encoding](https://github.com/jsdom/whatwg-encoding) which remove the `iconv-lite` and decode method. Only gave the mapping in the [Encoding Standard](https://encoding.spec.whatwg.org/).

Data is from https://encoding.spec.whatwg.org/encodings.json

```js
const whatwgEncoding = require("whatwg-encoding-mapper");

console.assert(whatwgEncoding.labelToName("latin1") === "windows-1252");
console.assert(whatwgEncoding.labelToName("  CYRILLic ") === "ISO-8859-5");

console.assert(whatwgEncoding.isSupported("IBM866") === true);

// Not supported by the Encoding Standard
console.assert(whatwgEncoding.isSupported("UTF-32") === false);

// In the Encoding Standard, but this package can't decode it
console.assert(whatwgEncoding.isSupported("x-mac-cyrillic") === false);

console.assert(whatwgEncoding.getBOMEncoding(Buffer.from([0xFE, 0xFF])) === "UTF-16BE");
console.assert(whatwgEncoding.getBOMEncoding(Buffer.from([0x48, 0x69])) === null);
```

## API

- `labelToName(label)`: performs the [get an encoding](https://encoding.spec.whatwg.org/#concept-encoding-get) algorithm and returns the resulting encoding's name, or `null` for failure
- `isSupported(name)`: returns whether the encoding is one of [the encodings](https://encoding.spec.whatwg.org/#names-and-labels) of the Encoding Standard, _and_ is an encoding that this package can decode (via iconv-lite)
- `getBOMEncoding(buffer)`: sniffs the first 2–3 bytes of the supplied `Buffer`, returning one of the encoding names `"UTF-8"`, `"UTF-16LE"`, or `"UTF-16BE"` if the appropriate BOM is present, or `null` if no BOM is present

## Unsupported encodings

if you use `iconv-lite` out of this package. check the [document](https://github.com/ashtuchkin/iconv-lite/wiki/Supported-Encodings) for updated and more accurate result.

supportedName list is generated using the `iconv-lite` in `devDependencies` setction in `package.json`

## Credits

`whatwg-encoding` was originally based on the excellent work of [@nicolashenry](https://github.com/nicolashenry), [in jsdom](https://github.com/tmpvar/jsdom/blob/7ce11776ce161e8d5921a7a183585327400f786b/lib/jsdom/living/helpers/encoding.js). It has since been pulled out into this separate package.

## Alternatives

If you are looking for a JavaScript implementation of the Encoding Standard's `TextEncoder` and `TextDecoder` APIs, you'll want [@inexorabletash](https://github.com/inexorabletash)'s [text-encoding](https://github.com/inexorabletash/text-encoding) package.
