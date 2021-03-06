Smoke
=====
Smoke is a JavaScript mocking and stubbing framework. It has a familiar RSpec style interface and whilst it is perfectly capable of being used as a free-standing tool it comes complete with a plugin for the screw-unit testing framework.
Below are some getting started guides to get you on your way. See the included specs for further help.

Getting Started (With Screw.Unit)
---------------------------------
See `spec/suite.html` and the test file `screw_integration_spec.js` for lots of real world usage examples.

but you will need to include the Smoke files and you can get straight to work using the mock() and spec() methods...

	foo = {bar: function(attribute){return 'hello'}, baz:'goodbye'};
	stub(foo, 'bar').and_return('hi!');
	stub(foo, 'baz').and_set_to('welcome');
	myMock = mock('test');
	myMock.should_receive('foo').at_least('once').and_return('hello');
	expect(foo.baz).to(equal, 'hi!');

Additionally Smoke now has a macro for mocking anonymous functions this is great for mocking and stubbing closures...

	callback = mock_function();
	callback.should_be_invoked().exactly('once');
	$.bind('click', callback);
	$.trigger('click');

Ajax mocking (With Screw.Unit)
-----------------------------

It's as easy as Smoke.Ajax.mock("/giveMeNiceResponseBaby", "Nice Response", 200); which will mock response for given url. In Screw.Unit you need to also use wait() (available in [rsutphin's fork](http://github.com/rsutphin/screw-unit/tree/master)).

Example:

    Screw.Unit(function() {
      describe("mocking ajax", function() {
        it("allows mocking ajax request", function() {
          Smoke.Ajax.mock("/giveMeNiceResponseBaby", "Nice Response", 200);
          var data, textStatus;
          $.ajax({
            url: "/giveMeNiceResponseBaby",
            success: function(data_, textStatus_) {
              data = data_;
              textStatus = textStatus_;
          }});
          wait(function() {
            expect(data).to(equal, "Nice Response");
            expect(textStatus).to(equal, "success");
          }, 50);
        });
      });
    });

Getting Started (Free Standing)
-------------------------------
Include the library files in your document...

	<!-- require smoke -->
	<script src="../lib/smoke.mock.js"></script>
	<script src="../lib/smoke.stub.js"></script>

Create your stubs...

	foo = {bar: function(attribute){return 'hello'}, baz:'goodbye'};
	new Smoke.Stub(foo,'baz').and_set_to('boo');
	new Smoke.Stub(foo,'bar').and_return('blah');

Create your mocks...

	var myMock = Smoke.Mock();
	var myMockFunction = Smoke.MockFunction();

Create your expectations...

	myMock.should_receive('foo').with('bar').exactly(3, 'times').and_return('baz');

Check you expectations...

	Smoke.checkExpectations();

More About Mocks
----------------
Mocks are the main part of the Smoke framework. But Smoke.Mock() has a bit of a dual personality depending on if you pass it a value.

1. *without any arguments* it will return a fresh Mock with no more than default Object methods. You will need to mock all your interactions and check all your expectations.
2. *with an argument* it will return a 'Mocked' version of the object that was passed in. This is very useful if you just want to mock a single method on your object whilst leaving the rest intact. It's especially helpful for just carrying out expectations on existing objects without any mocking at all.

Smoke expectations are now destructive meaning that if you add an expectation then the previous method will not be invoked (if one exists) and undefined will be returned (unless you specify otherwise using and_return()).

Known Issues
------------
* Whilst you can stub both functions and properties you can currently only carry out mock expectations on functions. Unfortunately I think this is a language limitation of JavaScript but if anyone has any bright ideas then I'm all ears.

Contact
-------
Please send patches, comments or suggestions to andrew.d.kent@gmail.com
