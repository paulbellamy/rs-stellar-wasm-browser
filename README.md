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
await init();

// A simple factorial contract with a method `invoke(i64) -> i64`
const FACTORIAL_WASM = "AGFzbQEAAAABBgFgAX4BfgMCAQAFAwEAAQYIAX8BQYCABAsHEwIGbWVtb3J5AgAGaW52b2tlAAAKNwE1AQJ+QgAhASAAQgAgAEIAVRshAkIBIQACQANAIAIgAVENASAAIAFCAXwiAX4hAAwACwsgAAs=";

// Args are passed in as base64-encoded xdr
const args: StellarBase.xdr.ScVal[] = [
  StellarBase.xdr.ScVal.scvPosI64(
    StellarBase.xdr.Int64.fromString("1")
  )
];
const argsXdr = StellarBase.xdr.ScVec.toXDR(args).toString('base64');

// Call the contract.invoke method
const resultXdr = Buffer.from(invoke_contract(FACTORIAL_WASM, "invoke", argsXdr));

// Decode the result xdr
const result = StellarBase.xdr.ScVal.fromXDR(resultXdr, 'base64').posI64();
```

## TODO

- [ ] Figure out and load the needed ledger current state data
- [ ] Implement host function
- [ ] Dynamically load the contract WASM
- [ ] Nicer result formatting/display
- [ ] Figure out how big this is once compiled with the host functions etc.
  - Currently the runtime is ~440kb, 192kb gzipped.
