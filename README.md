# Go Vendor Do What I Want

GVDWIW is a no-frills tool for fixing dependencies of Go projects. It:

- Records versions of all packages that your project uses, explicitly or
  transitively (via other packages).
- Figures out what these versions are without user input.
- Restores these dependent packages to these versions on demand.
- Does not require storing source code of dependent packages in your
  project's repository.
- Does not require your project to be in GOPATH.
- Assumes every project has its own GOPATH.

## Operation

GVDWIW assumes that each Go project has its own GOPATH.
GVDWIW then provides two commands:

- `go-vendor-dwiw save` recursively walks the current
GOPATH and records current commit hashes for all Git repositories
in the current project's `Godeps` file.
- `go-vendor-dwiw restore` recursively walks the current GOPATH and
checks out commit hashes specified in `Godeps` for all Git repositories
in GOPATH.

## Caveats

GVDWIW does not install dependencies. Dependencies can be installed using
any mechanism you like. GVDWIW works well with
[go-get-deps](https://github.com/p-vital/go-get-deps).

GVDWIW requires dependencies to be installed before restoring fixed versions.
`go-vendor-dwiw restore` only restores versions of packages that are
already installed in GOPATH.

GVDWIW does not add dependencies to the project's source control repository,
nor does GVDWIW require such addition.

## Limitations

If your project depends on package A which depends on package B,
and at a later time A is updated such that it no longer depends on B,
a subsequent installation of A won't install B and when
`go-vendor-dwiw restore` runs it will simply ignore the version specification
for B.

## Godeps Format

The format of `Godeps` file is simple: each line contains the Git
repository for a package followed by commit hash of the desired version.
Example:

	gopkg.in/check.v1 4f90aeace3a26ad7021961c297b22c42160c7b25

There is no provision for comments at this time.
`Godeps` is not meant to be edited by humans.

## License

Released under the MIT license.
