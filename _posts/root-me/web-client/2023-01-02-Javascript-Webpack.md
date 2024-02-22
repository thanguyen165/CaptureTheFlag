---
title: Javascript - Webpack
author: thanguyen165
date: 2023-01-02 07:00:00 +0700
categories: [Practice, rootme]
tags: [Web Exploitation, rootme]
---

* Points: 15
* Link: [https://www.root-me.org/en/Challenges/Web-Client/Javascript-Webpack](https://www.root-me.org/en/Challenges/Web-Client/Javascript-Webpack)

## Description
> Do you know webpack?

## Statement
> Find the password.

## Solution

Firstly, get the page's source code:
```sh
curl http://challenge01.root-me.org/web-client/ch27/
```

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset=utf-8>
    <meta name=viewport content="width=device-width,initial-scale=1">
    <title>Javascript - Webpack</title>
    <link href=./static/css/app.f213bed71a5d27ded35fdf00a1963840.css rel=stylesheet>
  </head>
  <body>
    <link rel='stylesheet' property='stylesheet' id='s' type='text/css' href='/template/s.css' media='all' />
    <iframe id='iframe' src='https://www.root-me.org/?page=externe_header'></iframe>
    <div id=app></div>
    <script type=text/javascript src=./static/js/manifest.2ae2e69a05c33dfc65f8.js></script>
    <script type=text/javascript src=./static/js/vendor.458c9f5863b8f28e5570.js></script>
    <script type=text/javascript src=./static/js/app.a92c5074dafac0cb6365.js></script>
  </body>
</html>
```

Let's check the .js file:
```sh
curl http://challenge01.root-me.org/web-client/ch27/static/js/app.a92c5074dafac0cb6365.js
```

```javascript
webpackJsonp([0], {
    "0qD8": function(t, a) {},
    Gz79: function(t, a) {},
    JAfz: function(t, a) {},
    NHnr: function(t, a, n) {
        "use strict";
        Object.defineProperty(a, "__esModule", {
            value: !0
        });
        var s = n("7+uW"),
            i = {
                render: function() {
                    var t = this.$createElement,
                        a = this._self._c || t;
                    return a("div", {
                        attrs: {
                            id: "app"
                        }
                    }, [a("div", {
                        staticClass: "wrapper"
                    }, [a("header", {
                        staticClass: "site-header"
                    }, [a("nav", [a("strong", [this._v("Quack Quack ! | ")]), this._v(" "), a("router-link", {
                        attrs: {
                            to: "/duck"
                        }
                    }, [this._v("Duck")]), this._v(" or\n        "), a("router-link", {
                        attrs: {
                            to: "/duck-mandarin"
                        }
                    }, [this._v("Mandarin duck")])], 1)]), this._v(" "), a("router-view")], 1)])
                },
                staticRenderFns: []
            };
        var e = n("VU/8")({
                name: "App"
            }, i, !1, function(t) {
                n("Ui3U")
            }, null, null).exports,
            c = n("/ocq"),
            r = {
                render: function() {
                    this.$createElement;
                    this._self._c;
                    return this._m(0)
                },
                staticRenderFns: [function() {
                    var t = this.$createElement,
                        a = this._self._c || t;
                    return a("main", [a("div", {
                        staticClass: "home-page"
                    }, [a("div", {
                        staticClass: "block"
                    }, [a("h1", [this._v("Welcome ! ")]), this._v(" "), a("p", {
                        staticClass: "intro"
                    }, [this._v("If you are here, it means that you don't know the difference between a duck and mandarin duck.")]), this._v(" "), a("h2", [this._v("Shame on you !")])]), this._v(" "), a("div", {
                        staticClass: "block"
                    })])])
                }]
            };
        var u = n("VU/8")({
                name: "Home",
                data: function() {
                    return {}
                }
            }, r, !1, function(t) {
                n("JAfz")
            }, "data-v-5c6c3158", null).exports,
            o = {
                render: function() {
                    this.$createElement;
                    this._self._c;
                    return this._m(0)
                },
                staticRenderFns: [function() {
                    var t = this.$createElement,
                        a = this._self._c || t;
                    return a("main", [a("div", {
                        staticClass: "home-page"
                    }, [a("div", {
                        staticClass: "block"
                    }, [a("h1", [this._v("This a normal duck ! ")]), this._v(" "), a("p", {
                        staticClass: "intro"
                    }, [this._v("It is just a duck... You can eat this one.")])]), this._v(" "), a("div", {
                        staticClass: "block"
                    }, [a("img", {
                        attrs: {
                            src: "static/duck.png"
                        }
                    })])])])
                }]
            };
        var h = n("VU/8")({
                name: "Duck",
                data: function() {
                    return {}
                }
            }, o, !1, function(t) {
                n("Gz79")
            }, "data-v-1a7d2945", null).exports,
            d = {
                render: function() {
                    this.$createElement;
                    this._self._c;
                    return this._m(0)
                },
                staticRenderFns: [function() {
                    var t = this.$createElement,
                        a = this._self._c || t;
                    return a("main", [a("div", {
                        staticClass: "home-page"
                    }, [a("div", {
                        staticClass: "block"
                    }, [a("h1", [this._v("This a mandarin duck ! ")]), this._v(" "), a("p", {
                        staticClass: "intro"
                    }, [this._v("It is a mandarin duck !!!!! As you can see, it is much more beautiful than a normal duck.")]), this._v(" "), a("h2", [this._v("DO NOT EAT THIS ONE !")])]), this._v(" "), a("div", {
                        staticClass: "block"
                    }, [a("img", {
                        attrs: {
                            src: "static/duck-mandarin.png"
                        }
                    })])])])
                }]
            };
        var l = n("VU/8")({
                name: "Duck",
                data: function() {
                    return {}
                }
            }, d, !1, function(t) {
                n("0qD8")
            }, "data-v-bb84558a", null).exports,
            v = {
                render: function() {
                    this.$createElement;
                    this._self._c;
                    return this._m(0)
                },
                staticRenderFns: [function() {
                    var t = this.$createElement,
                        a = this._self._c || t;
                    return a("main", [a("div", {
                        staticClass: "home-page"
                    }, [a("div", {
                        staticClass: "block"
                    }, [a("h1", [this._v("This a mandarin duck ! ")]), this._v(" "), a("p", {
                        staticClass: "intro"
                    }, [this._v("It is a mandarin duck !!!!! As you can see, it is much more beautiful than a normal duck.")]), this._v(" "), a("h2", [this._v("DO NOT EAT THIS ONE !")])]), this._v(" "), a("div", {
                        staticClass: "block"
                    }, [a("img", {
                        attrs: {
                            src: "/static/duck-mandarin.png"
                        }
                    })])])])
                }]
            };
        n("VU/8")({
            name: "Duck",
            data: function() {
                return {
                    msg: "Quack quack !! :)."
                }
            }
        }, v, !1, function(t) {
            n("TNCB")
        }, "data-v-4b9752b8", null).exports;
        s.a.use(c.a);
        var m = new c.a({
            routes: [{
                path: "/",
                name: "Home",
                component: u
            }, {
                path: "/duck",
                name: "Duck",
                component: h
            }, {
                path: "/duck-mandarin",
                name: "DuckMandarin",
                component: l
            }]
        });
        s.a.config.productionTip = !1, new s.a({
            el: "#app",
            router: m,
            components: {
                App: e
            },
            template: "<App/>"
        })
    },
    TNCB: function(t, a) {},
    Ui3U: function(t, a) {}
}, ["NHnr"]);
//# sourceMappingURL=app.a92c5074dafac0cb6365.js.map
```

The file in the comment is suspected, let's check it
```sh
curl http://challenge01.root-me.org/web-client/ch27/static/js/app.a92c5074dafac0cb6365.js.map
```

The file is so long, but it has this line:

```
Here is your flag : BecauseSourceMapsAreGreatForDebuggingButNotForProduction
```

The flag is:
```
BecauseSourceMapsAreGreatForDebuggingButNotForProduction
```
