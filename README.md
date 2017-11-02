# bz.js! 

### [![Build Status](https://travis-ci.org/canuckistani/bz.js.png)](https://travis-ci.org/canuckistani/bz.js)

*A JavaScript wrapper for the [Bugzilla REST API](https://wiki.mozilla.org/Bugzilla:REST_API)*.

## Warning!!!

Although Travis tests are now working, test coverage is far from complete. Please [report issues](https://github.com/canuckistani/bz.js/issues) especially if you find problems running bz.js in the browser. Worst case scenario, please revert to [0.3](https://github.com/canuckistani/bz.js/tree/0.3x) as it still basically works.

## Install
For [node](http://nodejs.org) install with [npm](http://npmjs.org):

```
npm install bz
```

and use with `var bz = require("bz")`

For the browser, download the latest from https://unpkg.com/bz 

## Development

1. git clone git@github.com:canuckistani/bz.js.git
2. cd ./bz.js
3. npm install

### Builds

- `npm run build` - this will build node and browser files from the ./src directory. The node main entry points to `./build`.

### Tests

Some tests are included. If you want to run the tests, you can do so with `npm test`.

## Usage

```javascript
var bugzilla = bz.createClient();

bugzilla
  .getBug(678223)
  .then(bug => {
    console.log(bug.summary);
  });
```

# API
`bz.createClient(options)`
creates a new Bugzilla API client, optionally takes options like the REST API url, username + password or apiKey:

```javascript
var bugzilla = bz.createClient({
  url: 'https://api-dev.bugzilla.mozilla.org/rest/',
  username: 'bugs@bugmail.com',
  password: 'secret'
});
```

```javascript
var bugzilla = bz.createClient({
  url: 'https://api-dev.bugzilla.mozilla.org/rest/',
  apiKey: '23dsf3423s',
  timeout: 30000
});
```

See docs on the [login][bugzilla-login] endpoint.

### Client methods
Each method returns a Promise so you can await on the data or catch any errors.

`getBug(id)`  
retrieves a [bug](https://wiki.mozilla.org/Bugzilla:REST_API:Objects#Bug) given a bug id.

`searchBugs(searchParams)`  
searches with given [search parameters](https://wiki.mozilla.org/Bugzilla:REST_API:Search) and fetches an array of [bugs](https://wiki.mozilla.org/Bugzilla:REST_API:Objects#Bug).

`countBugs(searchParams)`  
<del>searches with given [search parameters](https://wiki.mozilla.org/Bugzilla:REST_API:Search) and gets a integer count of bugs matching that query.</del> this is not supported currently.

`createBug(bug)`  
creates a [bug](https://wiki.mozilla.org/Bugzilla:REST_API:Objects#Bug) and returns the id of the newly created bug.

`updateBug(id, bug)`  
updates a [bug](https://wiki.mozilla.org/Bugzilla:REST_API:Objects#Bug) with new bug info.

`bugComments(id)`  
retrieves the [comments](https://wiki.mozilla.org/Bugzilla:REST_API:Objects#Comment) for a bug.

`addComment(id, comment)`  
adds a [comment](https://wiki.mozilla.org/Bugzilla:REST_API:Objects#Comment) to a bug.

`bugHistory(id)`  
retrieves array of [changes](https://wiki.mozilla.org/Bugzilla:REST_API:Objects#ChangeSet) for a bug.

`bugFlags(id)`  
retrieves array of [flags](https://wiki.mozilla.org/Bugzilla:REST_API:Objects#Flag) for a bug.

`bugAttachments(id)`  
retrieves array of [attachments](https://wiki.mozilla.org/Bugzilla:REST_API:Objects#Attachment) for a bug.

`createAttachment(bugId, attachment)`  
creates an [attachment](https://wiki.mozilla.org/Bugzilla:REST_API:Objects#Attachment) on a bug, and returns the id of the newly created attachment.

`getAttachment(attachId)`  
gets an [attachment](https://wiki.mozilla.org/Bugzilla:REST_API:Objects#Attachment) given an attachment id.

`updateAttachment(attachId, attachment)`  
updates the [attachment](https://wiki.mozilla.org/Bugzilla:REST_API:Objects#Attachment).

`searchUsers(match)`  
searches for [users](https://wiki.mozilla.org/Bugzilla:REST_API:Objects#User) by string, matching against users' names or real names.
 
`getUser(userId)`  
retrieves a [user](https://wiki.mozilla.org/Bugzilla:REST_API:Objects#User) given a user id.

`getSuggestedReviewers(id)`  
retrieves a list of [suggested reviewers](https://wiki.mozilla.org/Bugzilla:BzAPI:Objects#Suggested_Reviewer) for a bug.

`getConfiguration(options)`  
gets the [configuration](https://wiki.mozilla.org/Bugzilla:REST_API:Objects:Configuration) of this Bugzilla server. Note: this only works currently against Mozilla's production instance, the bugzilla 5.0 instance running on landfill has no equivalent call that can be run from the browser due to a lack of CORS headers.

[bugzilla-login]: https://wiki.mozilla.org/Bugzilla:REST_API
