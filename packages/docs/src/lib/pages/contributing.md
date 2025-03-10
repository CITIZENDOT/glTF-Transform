---
title: Contributing | glTF Transform
snippet: This project consists of multiple NPM packages, managed in one repository with Lerna. All code, excluding Node.js-based tests, is written in TypeScript…
---

# Contributing

This project consists of multiple NPM packages, managed in one repository with
[Lerna](https://lerna.js.org/). All code, excluding Node.js-based tests, is written in TypeScript.
I recommend using VSCode for linting and type information, which becomes especially helpful
when dealing with glTF schema objects.

After cloning the repository, run:

```bash
yarn install
```

The project relies on [Yarn workspaces](https://classic.yarnpkg.com/docs/workspaces/) and will not build with npm. To build and test all code,
run:

```bash
# Build on MacOS or Linux
yarn run dist

# Build on Windows, see https://github.com/donmccurdy/glTF-Transform/pull/959
yarn run dist --ignore "@gltf-transform/docs"

# Test
yarn test
```

To run an arbitrary command across all packages:

```bash
lerna exec -- <command>
```

While working, use `yarn run watch` to watch and rebuild code after changes. To use a local
version of the CLI, run `yarn link` within the `packages/cli` directory. Then
`gltf-transform -h` will use local code instead of any global installation.

### Pull requests

Before adding new features or packages to the repository, please open an issue on GitHub to discuss
your proposal. Some features may not fit the current scope of the project, or may be more than I am
able to maintain long-term. Even if a feature does not end up in this repository, custom
extensions and functions can be defined and used externally. Changes including test coverage are
strongly preferred.

New features should be compatible with both Node.js and Web, though exceptions may be possible in
certain situations. To accomplish that, some platform-specific resources (like instances of
[HTMLCanvasElement](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)) are passed into
API functions by the user, rather than being created by the API directly.

### Dependencies

I recommend compiling with Node.js v12.x, which is the latest version with a prebuilt binary for
`gl` as of April 2020.

Runtime dependencies should be installed only to the sub-package in which they are needed. Any
devDependencies are shared across all packages, and should be installed in the project root. Pull
requests should omit any changes to `dist/*` artifacts.

### Documentation

Documentation and examples are written in JSDoc comments on the relevant classes and methods,
processed with [TypeDoc](https://typedoc.org/), and rendered to a custom TypeDoc theme. Additions
and clarification are welcome, and examples may be added inline alongside class documentation.
Certain JSDoc tags have notable meanings within this project:

- `@internal` methods and classes are (1) hidden from documentation, and (2) not included in
  TypeScript definitions for the package. This code is intended only for use within the defining
  package.
- `@hidden` methods and classes are hidden from documentation, but still included in TypeScript
  definitions for the package. This code is not intended for wide use, but may be necessary for
  other packages in the glTF Transform monorepo.

### Roadmap

glTF Transform supports the complete glTF 2.0 core format, and some extensions. Most official Khronos Group extensions (those prefixed with `KHR_`) will be implemented on a rolling basis — pull requests are very welcome. Multi-vendor extensions (prefixed with `EXT_`) may be included on a case-by-case basis. Single-vendor extensions (any other prefix) are unlikely to be included directly in the project, although glTF Transform does provide APIs to build and maintain implementations for those extensions externally.

Suggestions and PRs for new [Functions](/functions) are also generally welcome.

### Releasing

> NOTE: Only the maintainer can create new releases.

All packages are published together. To create a standard release:

```bash
lerna publish [ patch | minor | major ] --force-publish "*"
```

To create an alpha release:

```bash
lerna publish prerelease --dist-tag next --force-publish "*"
```

If a release contains a new package, `-- --access public` must be appended. Lerna can be [finicky with 2FA OTPs](https://github.com/lerna/lerna/issues/1091).
