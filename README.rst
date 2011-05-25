
Utilities for testing jQuery-based JavaScript code.

Install
=======

Just include tests.js before running tests.  It was extracted from a Qunit_
based test suite where it gets included in a script tag in the HTML.
It requires jQuery_.

.. _Qunit: http://docs.jquery.com/Qunit
.. _jQuery: http://jquery.com/ 

Usage
=====

tests.waitFor(callable, config)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Wait for a condition before doing anything else.

Good for making async tests fast on fast machines.
Use it like this::

  tests.waitFor(function() {
      return (thing == 'done);
  }).thenDo(function() {
      equals(1,1);
      ok(stuff());
  });

You can pass in a config object as the second argument
with these possible attributes:

**config.interval**
  milliseconds to wait between polling condition
**config.timeout**
  milliseconds to wait before giving up on condition

tests.createSandbox(tmpl)
~~~~~~~~~~~~~~~~~~~~~~~~~

Returns a jQuery object for a temporary, unique div filled with html
from a template.

**tmpl**
  An optional jQuery locator from which to copy html.  This would
  typically be the ID of a div in your test runner HTML (e.g. qunit.html)

Example::

    module('Group of tests', {
        setup: function() {
            this.sandbox = tests.createSandbox('#foobar-template');
        },
        teardown: function() {
            this.sandbox.remove();
        }
    });

    test('some test', function() {
        this.sandbox.append('...');
    });

tests.StubOb(Orig, overrides)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Returns a class-like replacement for Orig.

**Orig**
  object you want to replace.
**overrides**
  object containing properties to override in the original.

All properties in the overrides object will replace those of Orig's
prototype when you create an instance of the class.  This is useful
for stubbing out certain methods.  For example::

    z.FileData = tests.StubOb(z.FormData, {
        send: function() {}
    });

This allows the creation of a FormData object that behaves just like
the original except that send() does not actually post data during
a test::

    var fileData = new z.FileData();
    fileData.send(); // does nothing

Be sure to assign the original class back when you're done testing.

tests.hasClass($sel, cls)
~~~~~~~~~~~~~~~~~~~~~~~~~

Asserts that jQuery selector $sel has CSS class cls.

Otherwise, an instructive error is raised.

tests.lacksClass($sel, cls)
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Asserts that jQuery selector $sel does not have CSS class cls.

Otherwise, an instructive error is raised.
