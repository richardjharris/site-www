---
title: Dart 2 migration guide for web apps
description: Tips for migrating your web app from Dart 1.x to Dart 2.
---

<style>
del { color: rgba(255,0,0,.35); }
del code { color: darkred; }
</style>

This page has information on migrating your Dart 1.x web app to Dart 2.
These changes are necessary because of the following:

- [Tooling changes](#tools):
  - **Chrome** replaces Dartium and content-shell.
  - A **new build system** replaces `pub build`, `pub serve`, pub transformers.
- Dart 2 [language and library changes.][dart-2]


## Tools

The development environment for web apps is different in Dart 2 from Dart 1.x.
Here are the highlights:

{:.table .table-striped}
| **Dart 1.x** | **Dart 2** |
| Dartium, content shell | Chrome and [dartdevc][] |
| `pub build` | [`webdev build`](/tools/webdev#build) |
| `pub serve` | [`webdev serve`](/tools/webdev#serve) |
| `pub run angular_test` | `pub run build_runner test -- -p chrome`. |
| pub transformers | [build][] package transformers. See: [Transforming code][] |

## Code

To migrate to Dart 2, you'll need to edit your web app's project files:

- `pubspec.yaml`. [See details below.](#pubspec)
- HTML files with `<script src="foo.dart"...>` elements,
  such as `web/index.html`. [See details below.](#web-index-html)
- Dart code, due to changes in the [Dart language and libraries.][dart-2]

For complete examples of migrating apps,
look at the files changed between the `4.x` and `master` branches
of the following apps:

- [Quickstart][angular-examples/quickstart]
- [Tour of Heroes, part 5][angular-examples/toh-5]

### Pubspec

Make these changes to your `pubspec.yaml` file:

- Add new `dev_dependencies`:
  - `build_runner: {{site.data.pubspec.dev_dependencies.build_runner}}`
  - `build_test: {{site.data.pubspec.dev_dependencies.build_test}}`, if you are running tests
  - `build_web_compilers: {{site.data.pubspec.dev_dependencies.build_web_compilers}}`
- Drop `dev_dependencies`:
  - <del>`browser`</del>
  - <del>`dart_to_js_script_rewriter`</del>
- Upgrade to `test` version 0.12.30 or later; it runs Chrome tests headless by default.
- Drop all `transformers`:
  - <del>`angular`</del>
  - <del>`dart_to_js_script_rewriter`</del>
  - <del>`test/pub_serve`</del>

For example, look at the differences in
the [Quickstart example's pubspec][angular-examples/quickstart/pubspec.yaml]
with these changes applied.

<a id="web-index-html"></a>
### HTML with script elements

The most common example of a file with `<script>` elements is `web/index.html`.
You'll need to make these changes:

- Drop <del>`<script defer src="packages/browser/dart.js"></script>`</del>
- Replace <del>`<script defer src="foo.dart" type="application/dart"></script>`</del> by<br>
  `<script defer src="foo.dart.js"></script>`

For example, look at the differences in the [Quickstart example's
web/index.html page][angular-examples/quickstart/web/index.html]
with these changes applied.

## Additional resources

The [Dart 2 migration guide][dart-2] has
information about changes in Dart 2, and how to migrate your code from Dart 1.x.

[angular-examples/quickstart]: https://github.com/googlearchive/quickstart/compare/4.x...master
[angular-examples/quickstart/pubspec.yaml]: https://github.com/googlearchive/quickstart/compare/4.x...master#diff-8b7e9df87668ffa6a04b32e1769a33434999e54ae081c52e5d943c541d4c0d25
[angular-examples/quickstart/web/index.html]: https://github.com/googlearchive/quickstart/compare/4.x...master#diff-8f62b6ced28d3396b501d2e89a2e7cb761d16cd7dc977aebece03d4a5da5c24e
[angular-examples/toh-5]: https://github.com/googlearchive/toh-5/compare/4.x...master
[build]: https://github.com/dart-lang/build
[dart-2]: /dart-2
[dartdevc]: /tools/dartdevc
[Documentation changelog]: https://web.archive.org/web/20181003225323/https://webdev.dartlang.org/changelog
[Transforming code]: https://github.com/dart-lang/build/blob/master/docs/transforming_code.md
