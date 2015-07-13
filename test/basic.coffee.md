    exec = (require '../README')()
    assert = require 'assert'

    describe 'Basic', ->

      it 'should be thenable', (done) ->
        exec "/bin/true"
        .then -> done()
        .catch done
        null

      it 'should run `echo`', (done) ->
        exec "/bin/echo 'Hello world'"
        .then (stdout) ->
          assert.deepEqual stdout, 'Hello world\n'
          done()
        null

      it 'should handle errors', (done) ->
        exec "/bin/foo-bared"
        .catch ->
          done()
        null

      it 'should report output on stderr', (done) ->
        exec "/bin/echo 'Hello world' >&2", true
        .catch (error) ->
          done()
        null

      it 'should handle options', (done) ->
        exec "./true", cwd:'/bin'
        .then -> done()
        null

      it 'should handle options and stdout', (done) ->
        exec "./echo 'Hello world'", cwd:'/bin'
        .then (stdout )->
          assert.deepEqual stdout, 'Hello world\n'
          done()
        null

      it 'should handle options and stdout and stderr', (done) ->
        exec "./echo 'Hello world'", cwd:'/bin', true
        .then (stdout )->
          assert.deepEqual stdout, 'Hello world\n'
          done()
        null

      it 'should handle options and errors', (done) ->
        exec "./echo 'Hello world' >&2", cwd:'/bin', true
        .catch (error) ->
          done()
        null
