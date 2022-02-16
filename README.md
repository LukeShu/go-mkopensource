## Command line tool Docs
* [go-mkopensource](/cmd/go-mkopensource/README.md)
* [js-mkopensource](/cmd/js-mkopensource/README.md)

## Building

You may use `go get github.com/datawire/go-mkopensource`, clone the
repo and run `go build .`, or any of the other usual ways of building
a Go program; there is nothing special about `go-mkopensource`.

## Using as a library

The [`github.com/datawire/go-mkopensource/pkg/detectlicense`][detectlicense]
package is good at detecting the licenses in a file

[detectlicense]: https://pkg.go.dev/github.com/datawire/go-mkopensource/pkg/detectlicense

## Design

There are many existing packages to do license detection, such as
[go-license-detector][] or GitHub's [licensee][].  The reason these
are not used is that they are meant to be _informative_, they provide
"best effort" identification of the license.

`go-mkopensource` isn't meant to just be _informative_, it is meant to
be used for _compliance_, if it has any reason at all to be even a
little skeptical of a result, rather than returning its best guess, it
blows up in your face, asking a human to verify the result.

[go-license-detector]: https://github.com/go-enry/go-license-detector
[licensee]: https://github.com/licensee/licensee

## License scanning scripts

Folder `/build-aux` contains scripts to scan licenses for Go, Python 
and Node.Js. Script will generate both `DEPENDENCY_LICENSES.md` and 
`DEPENDENCIES.md`

The following environment variables are used to configure the 
application behaviour.

* `APPLICATION`: Required. Name of the application being scanned.
  It's used in the header of the license files.

* `BUILD_HOME` Required. Location of the root folder of the repo to 
  scan.

* `BUILD_TMP`: Required. Folder to use for storing temporary files.

* `GIT_TOKEN` Required. Git token with permissions to pull 
  repositories

* `GO_BUILDER` Required. Image to use for generating Python
  dependencies.

* `PYTHON_PACKAGES`: Optional. List of requirement.txt files to scan.
  Paths should be relative to `BUILD_HOME`.      
  Example:

  `export PYTHON_PACKAGES="./python/requirements.txt ./builder/requirements.txt"`

* `PYTHON_BUILDER`: Required. Image to use for generating Python 
  dependencies.

* `NPM_PACKAGES`: Optional. List of package.json and package-lock.json 
  files to scan. Paths should be relative to `BUILD_HOME`.  
  Example:

  `export NPM_PACKAGES="./tools/sandbox/grpc_web/package.json ./tools/sandbox/grpc_web/package-lock.json"`

* `NODE_VERSION`: Required when `NPM_PACKAGES` is defined. Version 
  of Node.JS to use when running npm dependency scan. Only valid
  version numbers (X.Y.Z) are allowed.  
  Example:

  `NODE_VERSION=10`

* `SCRIPTS_HOME`: Required. Location where `go-mkopensource` repo is 
  checked out, relative to  `BUILD_HOME`

To update license information files, set the environment variables 
described above and run `build-aux/generate.sh`
