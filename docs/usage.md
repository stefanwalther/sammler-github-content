Use ***sammler*** to fetch contents from GitHub to store them locally.

### General concepts

#### Source Definition
A **source definition** contains the following properties:

- `user` - The user on GitHub (e.g. `stefanwalther`). Mandatory.
- `repo` - Name of the repository (e.g. `sammler-test-repo1`). Mandatory.
- `ref` - The branch, defaults to `master`
- `path` - Path to fetch contents from (e.g. `dir-1` n sammler-test-repo1). Defaults to "".
- `recursive` - Whether to fetch contents recursively or not, defaults to false.

**Example:**

```js
var sourceDef = {
	"user": "stefanwalther",
	"repo": "sammler-test-repo1",
	"ref": "",
	"path": "dir-1",
	"recursive": true
}
```

#### GitHub Content

(TBD)


### Methods

#### `fetchContents( sourceDef, gitHubContents, targetDir )`

> Retrieve gitHub contents based on a source-definition and store the results to the given target directory.
> Basically calls .getContent() and then .saveContents()

**Parameters**

- `sourceDef` - see Source Definition
- `gitHubContents` - Retrieved gitHub contents
- `targetDir` - Local target directory.

#### `getContent( sourceDef )`

> Retrieve contents from GitHub, based on a single source definition.

**Parameters**
- {Object} `sourceDef` - Source definition object.

**Example**

```js
"use strict";
var SammlerGitHub = require( "./../../dist/" ).SammlerGitHub;

var sammlerGitHub = new SammlerGitHub();

var sourceDef = {
	user: "stefanwalther",
	repo: "sammler-test-repo1",
	path: "dir-1",
	ref: "master",
	recursive: true
};

sammlerGitHub.getContent( sourceDef )
	.then( function ( data ) {
		data.results.forEach( function ( item ) {
			console.log( "\t" + item.path );
		});
	});

// Results:
// 	dir-1/.gitkeep
// 	dir-1/a.md
// 	dir-1/b.md
// 	dir-1/images/a.png
// 	dir-1/images/b.png
// 	dir-1/images/sub-single/single.md
// 	dir-1/images/sub/x.md
// 	dir-1/images/sub/y.png
// 	dir-1/images/sub/z.png

```

#### saveContents( sourceDef, gitHubContents, targetDir)

```js
"use strict";
var SammlerGitHub = require( "./../../dist/" ).SammlerGitHub;
var path = require( "path" );
var del = require( "del" );

var sammlerGitHub = new SammlerGitHub();

var sourceDef = {
	user: "stefanwalther",
	repo: "sammler-test-repo1",
	path: "dir-1",
	ref: "master",
	recursive: true
};

var targetDir = path.join( __dirname, "./.content/" );

del( targetDir ).then( function () { // Clean the targetDir ...
	sammlerGitHub.getContent( sourceDef )
		.then( function ( data ) {
			sammlerGitHub.saveContents( sourceDef, data, targetDir )
				.then( function ( results ) {
					console.log("Saved all");
					results.forEach( function ( item ) {
						console.log( "Item saved: " + item );
					} )
				} )
		} );
} );
```
