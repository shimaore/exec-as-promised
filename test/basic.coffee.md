    exec = (require '../README')()

    describe 'Basic', ->

      it 'should be thenable', (done) ->
        t = exec "/bin/true"
        t
          .catch done
          .then -> done()
        null

      it 'should run `echo`', ->
        exec "/bin/echo 'Hello world'"

      it 'should handle errors', (done) ->
        t = exec "/bin/foo-bared"
        t
          .catch -> done()
        null
