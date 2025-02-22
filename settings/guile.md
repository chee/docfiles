# .guile

```scheme filename=".guile"
(use-modules
  (hoot compile)
  (hoot reflect)
  (wasm assemble)
  (wasm parse)
  (ice-9 readline)
  (ice-9 binary-ports))

(define *reflect-path* "/opt/homebrew/share/guile-hoot/js/js-runtime/reflect.wasm")
(define reflect-wasm (call-with-input-file *reflect-path* parse-wasm))

(activate-readline)
```
