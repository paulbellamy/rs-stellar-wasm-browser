# rs-stellar-wasm-browser

**EXPERIMENTAL**

Run the stellar wasmi runtime in wasm, so it can run the browser.

Build with `wasm-pack build --target web`. That will build a `pkg` directory
with some stuff in it.

Then, to use it from javascript:

```js
import StellarBase from "stellar-base";

// Load the wasm contract runtime we just built. You'll need to change the 
import init, {invoke_contract} from "../rs-stellar-wasm-browser/pkg/stellar_wasm_browser.js";

// A simple factorial contract with a method `invoke(i64) -> i64`
const FACTORIAL_WASM = "AGFzbQEAAAABBgFgAX4BfgMCAQAFAwEAAQYIAX8BQYCABAsHEwIGbWVtb3J5AgAGaW52b2tlAAAKNwE1AQJ+QgAhASAAQgAgAEIAVRshAkIBIQACQANAIAIgAVENASAAIAFCAXwiAX4hAAwACwsgAAs=";

const args: StellarBase.xdr.ScVal[] = [
  StellarBase.xdr.ScVal.scvPosI64(
    StellarBase.xdr.Int64.fromString("1")
  )
];
const argsXdr = StellarBase.xdr.ScVec.toXDR(args).toString('base64');

await init();
const resultXdr = Buffer.from(invoke_contract(FACTORIAL_WASM, "invoke", argsXdr));
const result = StellarBase.xdr.ScVal.fromXDR(resultXdr, 'base64').posI64();
```
