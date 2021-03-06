OmnICU Charter
==============

OmnICU is a new project whose objective is to solve the needs of clients who wish to provide client-side i18n for their products in resource-constrained environments.

OmnICU will be built from the start with several key design constraints:

1. Small and modular code.
2. Pluggable locale data.
3. Availability and ease of use in multiple programming languages.
4. Written by i18n experts to encourage best practices.

OmnICU will provide an [ECMA-402](https://www.ecma-international.org/publications/standards/Ecma-402.htm)-compatible API surface in the target client-side platforms, including the web platform, iOS, Android, WearOS, WatchOS, Flutter, and Fuchsia, supported in programming languages including Rust, JavaScript, Objective-C, Java, Dart, and C++.

## Frequently Asked Questions

### How do you pronounce "OmnICU"?

In the international phonetic alphabet (IPA): ˈɑmniaɪsiju. [Hear it](https://itinerarium.github.io/phoneme-synthesis/?w=/'ɑmniaɪsiju/) [spoken](http://ipa-reader.xyz/?text=%CB%88%C9%91mnia%C9%AAsiju&voice=Joanna).

### What will be the organizational structure of OmnICU?

The OmnICU sub-committee (OmnICU-SC), a sub-committee of the ICU technical committee (ICU-TC), will be composed of members of the Unicode Consortium.  The OmnICU-SC will make architectural decisions consistent with this charter, and guide the development and maintenance of the project.

OmnICU will have an independent code base from ICU, and will operate independently of the ICU-TC. It will need no support from the the core staff of the Unicode Consortium except an occasional announcement.

### Is OmnICU going to replace ICU?

No!

OmnICU is a new library to fill the growing need for on-device i18n across a variety of client-side platforms, including IoT, mobile, and web environments.  We hope OmnICU will eventually replace client-side solutions such as Closure i18n (goog.i18n) and Dart Intl.  ICU will continue to be the gold standard for internationalization on servers and higher-resource environments.

### Why make a new project instead of improving ICU?

Because client-side needs are fundamentally different from server-side needs.

ICU has a long history of contributors adding [cruft](http://site.icu-project.org/design/cpp#TOC-Cruft-Complication) to serve their specific use cases.  Over that time, ICU has been increasingly optimized for high performance on servers; code size and constrained memory usage have not been at the forefront of design decisions.

Improving ICU to be the go-to solution to solve all client-side use cases would amount to a major overhaul and rewrite of the software, including a complete overhaul of the data loading mechanism, a re-write of classes with large code size, disentanglement of Java class dependencies, and a refactor of singleton caches to reduce memory usage.  In addition to these each being multi-quarter efforts, they would need to be done in a way that is backwards-compatible and does not hurt performance on the server side.  Such an effort also would not solve the current need from clients like Fuchsia and Mozilla for a memory-safe Rust ICU.  At the end of the day, given the interconnectedness of ICU primitives, even with compile-time dead code elimination, we will never be able to get code size down to as low as if we wrote new code with that as a primary design constraint.

A new project allows us to build in the client-side needs from day 1, including small code size and pluggable data dependencies, in a way that works well on all of the target platforms, without introducing further cruft to ICU in the process.

### How will you actually implement OmnICU?

Internally, the first draft of OmnICU will be built in a [no_std Rust Environment](https://rust-embedded.github.io/book/intro/no-std.html), with plans to use either FFI or [WebAssembly](https://webassembly.org/) for porting the code to the other target platforms; however, we are leaving the door open for other options such as transpilation that can produce code directly.  For example, we expect Rust clang/llvm tools to stabilize in the future, which could be the basis of a potential transpilation effort.  We are also looking at a Lisp-like source that can generate high-level code.

### Why not call it "icu4rust"?

The implementation language is intended to be an abstract concept: an internal detail which could change over time.  The focus of OmnICU is on the clients: the web platform, Android, iOS, and the other platforms listed earlier.  Rust, in the context Fuchsia and Gecko, is just one of many clients.

### Won't this increase the maintenance burden?

I18n engineers currently need to maintain several half-baked client-side i18n solutions including Closure i18n and Dart Intl.  We hope OmnICU will be able to eventually replace those libraries, and it will also allow us to fulfill the needs of new clients that we are currently unable to support.

### Will ICU and OmnICU share any code?

Since C++ and Java are both target output languages of OmnICU, it is possible that certain core algorithms can be implemented in OmnICU first and then shipped under the hood with ICU4C/ICU4J.  In ICU4C, Rust could be statically built into the ICU4C shared object files, such that it is transparent to ICU4C clients.  This is a theoretical possibility that will need to be evaluated by the ICU-TC at a later date.

### Why not put it in the ICU repository governed directly by ICU-TC?

OmnICU will have some overlap of personnel with ICU, but the processes, builds, and release cycle will be run separately from ICU.  The ICU repository is closely tied to the ICU release processes, with each pull request running the ICU4C and ICU4J test suites, linked to Jira issues.
