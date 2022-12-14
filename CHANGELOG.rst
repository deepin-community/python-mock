4.0.3
-----

- Issue #42532: Remove unexpected call of ``__bool__`` when passing a
  ``spec_arg`` argument to a Mock.

- Issue #39966: Revert bpo-25597. :class:`unittest.mock.MagicMock` with
  wraps' set uses default return values for magic methods.

- Issue #41877: Mock objects which are not unsafe will now raise an
  AttributeError if an attribute with the prefix asert, aseert, or assrt is
  accessed, in addition to this already happening for the prefixes assert or
  assret.

- Issue #40126: Fixed reverting multiple patches in unittest.mock. Patcher's
  ``__exit__()`` is now never called if its ``__enter__()`` is failed.
  Returning true from ``__exit__()`` silences now the exception.

4.0.2
-----

- Issue #39915: Ensure :attr:`unittest.mock.AsyncMock.await_args_list` has
  call objects in the order of awaited arguments instead of using
  :attr:`unittest.mock.Mock.call_args` which has the last value of the call.
  Patch by Karthikeyan Singaravelan.

4.0.1
-----

- Remove the universal marker from the wheel.

4.0.0
-----

- No Changes from 4.0.0b1.

4.0.0b1
-------

- The release is a fresh cut of cpython's `4a686504`__. All changes to :mod:`mock`
  from that commit and before are included in this release along with the
  subsequent changes listed below.

  __ https://github.com/python/cpython/commit/4a686504eb2bbf69adf78077458508a7ba131667

- Issue #37972: Subscripts to the `unittest.mock.call` objects now receive
  the same chaining mechanism as any other custom attributes, so that the
  following usage no longer raises a `TypeError`:

  call().foo().__getitem__('bar')

  Patch by blhsing

- Issue #38839: Fix some unused functions in tests. Patch by Adam Johnson.

- Issue #39485: Fix a bug in :func:`unittest.mock.create_autospec` that
  would complain about the wrong number of arguments for custom descriptors
  defined in an extension module returning functions.

- Issue #39082: Allow AsyncMock to correctly patch static/class methods

- Issue #38093: Fixes AsyncMock so it doesn't crash when used with
  AsyncContextManagers or AsyncIterators.

- Issue #38859: AsyncMock now returns StopAsyncIteration on the exaustion of
  a side_effects iterable. Since PEP-479 its Impossible to raise a
  StopIteration exception from a coroutine.

- Issue #38163: Child mocks will now detect their type as either synchronous
  or asynchronous, asynchronous child mocks will be AsyncMocks and
  synchronous child mocks will be either MagicMock or Mock (depending on
  their parent type).

- Issue #38473: Use signature from inner mock for autospecced methods
  attached with :func:`unittest.mock.attach_mock`. Patch by Karthikeyan
  Singaravelan.

- Issue #38136: Changes AsyncMock call count and await count to be two
  different counters. Now await count only counts when a coroutine has been
  awaited, not when it has been called, and vice-versa. Update the
  documentation around this.

- Issue #37555: Fix `NonCallableMock._call_matcher` returning tuple instead
  of `_Call` object when `self._spec_signature` exists. Patch by Elizabeth
  Uselton

- Issue #37251: Remove `__code__` check in AsyncMock that incorrectly
  evaluated function specs as async objects but failed to evaluate classes
  with `__await__` but no `__code__` attribute defined as async objects.

- Issue #38669: Raise :exc:`TypeError` when passing target as a string with
  :meth:`unittest.mock.patch.object`.

- Issue #25597: Ensure, if ``wraps`` is supplied to
  :class:`unittest.mock.MagicMock`, it is used to calculate return values
  for the magic methods instead of using the default return values. Patch by
  Karthikeyan Singaravelan.

- Issue #38108: Any synchronous magic methods on an AsyncMock now return a
  MagicMock. Any asynchronous magic methods on a MagicMock now return an
  AsyncMock.

- Issue #21478: Record calls to parent when autospecced object is attached
  to a mock using :func:`unittest.mock.attach_mock`. Patch by Karthikeyan
  Singaravelan.

- Issue #38857: AsyncMock fix for return values that are awaitable types.
  This also covers side_effect iterable values that happend to be awaitable,
  and wraps callables that return an awaitable type. Before these awaitables
  were being awaited instead of being returned as is.

- Issue #38932: Mock fully resets child objects on reset_mock(). Patch by
  Vegard Stikbakke

- Issue #37685: Fixed ``__eq__``, ``__lt__`` etc implementations in some
  classes. They now return :data:`NotImplemented` for unsupported type of
  the other operand. This allows the other operand to play role (for example
  the equality comparison with :data:`~unittest.mock.ANY` will return
  ``True``).

- Issue #37212: :func:`unittest.mock.call` now preserves the order of
  keyword arguments in repr output. Patch by Karthikeyan Singaravelan.

- Issue #37828: Fix default mock name in
  :meth:`unittest.mock.Mock.assert_called` exceptions. Patch by Abraham
  Toriz Cruz.

- Issue #36871: Improve error handling for the assert_has_calls and
  assert_has_awaits methods of mocks. Fixed a bug where any errors
  encountered while binding the expected calls to the mock's spec were
  silently swallowed, leading to misleading error output.

- Issue #21600: Fix :func:`mock.patch.stopall` to stop active patches that
  were created with :func:`mock.patch.dict`.

- Issue #38161: Removes _AwaitEvent from AsyncMock.

- Issue #36871: Ensure method signature is used instead of constructor
  signature of a class while asserting mock object against method calls.
  Patch by Karthikeyan Singaravelan.

3.0.5
-----

- Issue #31855: :func:`unittest.mock.mock_open` results now respects the
  argument of read([size]). Patch contributed by R??mi Lapeyre.

3.0.4
-----

- Include the license, readme and changelog in the source distribution.

3.0.3
-----

- Fixed patching of dictionaries, when specifying the target with a
  unicode on Python 2.

3.0.2
-----

- Add missing ``funcsigs`` dependency on Python 2.

3.0.1
-----

- Fix packaging issue where ``six`` was missed as a dependency.

3.0.0
-----

- Issue #35226: Recursively check arguments when testing for equality of
  :class:`unittest.mock.call` objects and add note that tracking of
  parameters used to create ancestors of mocks in ``mock_calls`` is not
  possible.

- Issue #31177: Fix bug that prevented using :meth:`reset_mock
  <unittest.mock.Mock.reset_mock>` on mock instances with deleted attributes

- Issue #26704: Added test demonstrating double-patching of an instance
  method.  Patch by Anthony Sottile.

- Issue #35500: Write expected and actual call parameters on separate lines
  in :meth:`unittest.mock.Mock.assert_called_with` assertion errors.
  Contributed by Susan Su.

- Issue #35330: When a :class:`Mock` instance was used to wrap an object, if
  `side_effect` is used in one of the mocks of it methods, don't call the
  original implementation and return the result of using the side effect the
  same way that it is done with return_value.

- Issue #30541: Add new function to seal a mock and prevent the
  automatically creation of child mocks. Patch by Mario Corchero.

- Issue #35022: :class:`unittest.mock.MagicMock` now supports the
  ``__fspath__`` method (from :class:`os.PathLike`).

- Issue #33516: :class:`unittest.mock.MagicMock` now supports the
  ``__round__`` magic method.

- Issue #35512: :func:`unittest.mock.patch.dict` used as a decorator with
  string target resolves the target during function call instead of during
  decorator construction. Patch by Karthikeyan Singaravelan.

- Issue #36366: Calling ``stop()`` on an unstarted or stopped
  :func:`unittest.mock.patch` object will now return `None` instead of
  raising :exc:`RuntimeError`, making the method idempotent. Patch
  byKarthikeyan Singaravelan.

- Issue #35357: Internal attributes' names of unittest.mock._Call and
  unittest.mock.MagicProxy (name, parent & from_kall) are now prefixed with
  _mock_ in order to prevent clashes with widely used object attributes.
  Fixed minor typo in test function name.

- Issue #20239: Allow repeated assignment deletion of
  :class:`unittest.mock.Mock` attributes. Patch by Pablo Galindo.

- Issue #35082: Don't return deleted attributes when calling dir on a
  :class:`unittest.mock.Mock`.

- Issue #0: Improved an error message when mock assert_has_calls fails.

- Issue #23078: Add support for :func:`classmethod` and :func:`staticmethod`
  to :func:`unittest.mock.create_autospec`.  Initial patch by Felipe Ochoa.

- Issue #21478: Calls to a child function created with
  :func:`unittest.mock.create_autospec` should propagate to the parent.
  Patch by Karthikeyan Singaravelan.

- Issue #36598: Fix ``isinstance`` check for Mock objects with spec when the
  code is executed under tracing. Patch by Karthikeyan Singaravelan.

- Issue #32933: :func:`unittest.mock.mock_open` now supports iteration over
  the file contents. Patch by Tony Flury.

- Issue #21269: Add ``args`` and ``kwargs`` properties to mock call objects.
  Contributed by Kumar Akshay.

- Issue #17185: Set ``__signature__`` on mock for :mod:`inspect` to get
  signature. Patch by Karthikeyan Singaravelan.

- Issue #35047: ``unittest.mock`` now includes mock calls in exception
  messages if ``assert_not_called``, ``assert_called_once``, or
  ``assert_called_once_with`` fails. Patch by Petter Strandmark.

- Issue #28380: unittest.mock Mock autospec functions now properly support
  assert_called, assert_not_called, and assert_called_once.
  
- Issue #28735: Fixed the comparison of mock.MagickMock with mock.ANY.

- Issue #20804: The unittest.mock.sentinel attributes now preserve their
  identity when they are copied or pickled.

- Issue #28961: Fix unittest.mock._Call helper: don't ignore the name parameter
  anymore. Patch written by Jiajun Huang.

- Issue #26750: unittest.mock.create_autospec() now works properly for
  subclasses of property() and other data descriptors.

- Issue #21271: New keyword only parameters in reset_mock call.

- Issue #26807: mock_open 'files' no longer error on readline at end of file.
  Patch from Yolanda Robla.

- Issue #25195: Fix a regression in mock.MagicMock. _Call is a subclass of
  tuple (changeset 3603bae63c13 only works for classes) so we need to
  implement __ne__ ourselves.  Patch by Andrew Plummer.

2.0.0 and earlier
-----------------

- Issue #26323: Add Mock.assert_called() and Mock.assert_called_once()
  methods to unittest.mock. Patch written by Amit Saha.

- Issue #22138: Fix mock.patch behavior when patching descriptors. Restore
  original values after patching. Patch contributed by Sean McCully.

- Issue #24857: Comparing call_args to a long sequence now correctly returns a
  boolean result instead of raising an exception.  Patch by A Kaptur.

- Issue #23004: mock_open() now reads binary data correctly when the type of
  read_data is bytes.  Initial patch by Aaron Hill.

- Issue #21750: mock_open.read_data can now be read from each instance, as it
  could in Python 3.3.

- Issue #18622: unittest.mock.mock_open().reset_mock would recurse infinitely.
  Patch from Nicola Palumbo and Laurent De Buyst.

- Issue #23661: unittest.mock side_effects can now be exceptions again. This
  was a regression vs Python 3.4. Patch from Ignacio Rossi

- Issue #23310: Fix MagicMock's initializer to work with __methods__, just
  like configure_mock().  Patch by Kasia Jachim.

- Issue #23568: Add rdivmod support to MagicMock() objects.
  Patch by H??kan L??vdahl.

- Issue #23581: Add matmul support to MagicMock. Patch by H??kan L??vdahl.

- Issue #23326: Removed __ne__ implementations.  Since fixing default __ne__
  implementation in issue #21408 they are redundant. *** NOT BACKPORTED ***

- Issue #21270: We now override tuple methods in mock.call objects so that
  they can be used as normal call attributes.

- Issue #21256: Printout of keyword args should be in deterministic order in
  a mock function call. This will help to write better doctests.

- Issue #21262: New method assert_not_called for Mock.
  It raises AssertionError if the mock has been called.

- Issue #21238: New keyword argument `unsafe` to Mock. It raises
  `AttributeError` incase of an attribute startswith assert or assret.

- Issue #21239: patch.stopall() didn't work deterministically when the same
  name was patched more than once.

- Issue #21222: Passing name keyword argument to mock.create_autospec now
  works.

- Issue #17826: setting an iterable side_effect on a mock function created by
  create_autospec now works. Patch by Kushal Das.

- Issue #17826: setting an iterable side_effect on a mock function created by
  create_autospec now works. Patch by Kushal Das.

- Issue #20968: unittest.mock.MagicMock now supports division.
  Patch by Johannes Baiter.

- Issue #20189: unittest.mock now no longer assumes that any object for
  which it could get an inspect.Signature is a callable written in Python.
  Fix courtesy of Michael Foord.

- Issue #17467: add readline and readlines support to mock_open in
  unittest.mock.

- Issue #17015: When it has a spec, a Mock object now inspects its signature
  when matching calls, so that arguments can be matched positionally or
  by name.

- Issue #15323: improve failure message of Mock.assert_called_once_with

- Issue #14857: fix regression in references to PEP 3135 implicit __class__
  closure variable (Reopens issue #12370)

- Issue #14295: Add unittest.mock
