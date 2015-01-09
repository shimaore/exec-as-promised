exec as Promise(d)
==================

Runs Node.js' `exec` asynchronously, optionally logging and reporting non-empty `stderr` content.

Usage
-----

```javascript
exec = require('exec-as-promised')()
```

You may provide a logger (such as `console`, `winston`, ...):

```javascript
exec = require('exec-as-promised')(console)
```

    module.exports = (logger) ->
      (cmd,fail_if_stderr = false) ->
        logger?.info cmd
        _exec cmd
        .then ([stdout,stderr]) ->
          logger?.info 'Command returned', {cmd,stdout,stderr}
          throw new Error stderr if stderr and fail_if_stderr
          stdout
        .catch (error) ->
          logger?.error "#{cmd} failed: #{error}"
          throw error

    Promise = require 'bluebird'
    _exec = Promise.promisify (require 'child_process').exec
