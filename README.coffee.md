exec as Promise(d)
==================

Runs Node.js' `exec` asynchronously, optionally logging and reporting non-empty `stderr` content.

Usage
-----

```javascript
exec = require('exec-as-promised')()
```

Then `exec(command,[options],[fail_if_stderr])` where `options` is the same as in `child_process.exec`, and `fail_if_stderr` is a boolean indicating whether to reject if `stderr` is not empty.

```javascript
exec('/bin/date')
.then( function (stdout) {
  console.log(date);
}
```

    module.exports = ->
      (cmd,args...) ->
        fail_if_stderr = false
        options = {}
        for arg in args
          switch typeof arg
            when 'boolean'
              fail_if_stderr = arg
            when 'object'
              options = arg
            else
              throw new Error "Invalid #{typeof arg} argument"

        debug "Executing", {cmd, options}
        _exec cmd, options
        .then ([stdout,stderr]) ->
          debug 'Command returned', {cmd,stdout,stderr}
          if stderr and fail_if_stderr
            Promise.reject new Error stderr
          else
            stdout
        .catch (error) ->
          debug "#{cmd} failed: #{error}"
          Promise.reject error

    Promise = require 'bluebird'
    _exec = Promise.promisify (require 'child_process').exec, multiArgs:true
    pkg = require './package'
    debug = (require 'debug') pkg.name
