---
title: Browser Support
authors:
  - ije
---

# Browser Support

Aleph.js requires a modern browser to support [native ES module imports](https://caniuse.com/#feat=es6-module) and **dynamic imports** during **development**:
- Chrome >= 61
- Edge >= 16
- Firefox >= 60
- Safari >= 11
- Opera >= 48

## `nomodule.js` Polyfill

(**WIP**, planning to implement in **v0.3**)
<br>
To support older browsers like *IE11* in **production**, Aleph.js will create a polyfilled `nomodule.js` that use [system.js](https://github.com/systemjs/systemjs) to import modules.
