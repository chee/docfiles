# lisp

## quicklisp

```commonlisp filename=".sbclrc"
;; -*- mode: common-lisp; -*-
#-quicklisp
(let ((quicklisp-init
  (merge-pathnames "quicklisp/setup.lisp" (user-homedir-pathname))))
  (when (probe-file quicklisp-init)
    (load quicklisp-init)))
```

## clpm

```commonlisp filename=".config/clpm/sources.conf"
("quicklisp"
  :type :quicklisp
  :url "https://beta.quicklisp.org/dist/quicklisp.txt")
```

assumes clpm installed with INSTALL_ROOT=~/.local

```commonlisp filename=".config/common-lisp/source-registry.conf.d/20-clpm-client.conf"
;; -*- mode: common-lisp; -*-
(:directory #P"/Users/chee/.local/share/clpm/client/")
```

```commonlisp filename=".sbclrc"
(require "asdf")
#-clpm-client
(when (asdf:find-system "clpm-client" nil)
  ;; Load the CLPM client if we can find it.
  (asdf:load-system "clpm-client")
  (when (uiop:symbol-call :clpm-client '#:active-context)
    ;; If started inside a context (i.e., with `clpm exec` or `clpm bundle exec`),
    ;; activate ASDF integration
    (uiop:symbol-call :clpm-client '#:activate-asdf-integration)))
```

### add clpm to sbcli startup

```commonlisp filename=".sbclirc"
(require "asdf")
#-clpm-client
(when (asdf:find-system "clpm-client" nil)
  (asdf:load-system "clpm-client")
  (when (uiop:symbol-call :clpm-client '#:active-context)
    (uiop:symbol-call :clpm-client '#:activate-asdf-integration)))
(clpm-client:activate-context "default" :activate-asdf-integration t)
```
