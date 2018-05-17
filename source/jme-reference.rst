.. _jme:

JME
===

JME expressions are used by students to enter answers to algebraic questions, and by question authors to define variables. JME syntax is similar to what you'd type on a calculator.

.. _variable-names:

Variable names
***************

Variable names are case-insensitive, so ``y`` represents the same thing as ``Y``. 
The first character of a variable name must be an alphabet letter; after that, any combination of letters, numbers and underscores is allowed, with any number of ``'`` on the end.

**Examples**: 
    * ``x``
    * ``x_1``
    * ``time_between_trials``
    * ``var1``
    * ``row1val2``
    * ``y''``

``e``, ``i`` and ``pi`` are reserved names representing mathematical constants. They are rewritten by the interpreter to their respective numerical values before evaluation.

This screencast describes which variable names are valid, and gives some advice on how you should pick names:

.. raw:: html
    
    <iframe src="https://player.vimeo.com/video/167085662" width="640" height="360" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

.. _variable-annotations:

Variable name annotations
-------------------------

Names can be given annotations to change how they are displayed. The following annotations are built-in:

* ``verb`` – does nothing, but names like ``i``, ``pi`` and ``e`` are not interpreted as the famous mathematical constants.
* ``op`` – denote the name as the name of an operator — wraps the name in the LaTeX \operatorname command when displayed
* ``v`` or ``vector`` – denote the name as representing a vector — the name is displayed in boldface
* ``unit`` – denote the name as representing a unit vector — places a hat above the name when displayed
* ``dot`` – places a dot above the name when displayed, for example when representing a derivative
* ``m`` or ``matrix`` – denote the name as representing a matrix — displayed using a non-italic font

Any other annotation is taken to be a LaTeX command. For example, a name ``vec:x`` is rendered in LaTeX as ``\vec{x}``, which places an arrow above the name.

You can apply multiple annotations to a single variable.
For example, ``v:dot:x`` produces a bold *x* with a dot on top: :math:`\boldsymbol{\dot{x}}`.

.. _jme-data-types:

Data types
**********

.. data:: number

    Numbers include integers, real numbers and complex numbers. There is only one data type for all numbers.

    ``i``, ``e``, ``infinity`` and ``pi`` are reserved keywords for the imaginary unit, the base of the natural logarithm, ∞ and π, respectively.

    **Examples**: ``0``, ``-1``, ``0.234``, ``i``, ``e``, ``pi``

.. data:: boolean

    Booleans represent either truth or falsity. The logical operations and, or and xor operate on and return booleans.

    **Examples**: ``true``, ``false``

.. data:: string

    Use strings to create non-mathematical text. Either ``'`` or ``"`` can be used to delimit strings.

    You can escape a character by placing a single backslash character before it. 
    The following escape codes have special behaviour:

    ====== =========
    ``\n`` New-line
    ``\{`` ``\{``
    ``\}`` ``\}``
    ====== =========

    If you want to write a string which contains a mixture of single and double quotes, you can delimit it with triple-double-quotes or triple-single-quotes, to save escaping too many characters.

    **Examples**: ``"hello there"``, ``'hello there'``, ``""" I said, "I'm Mike's friend" """``

.. data:: list

    An ordered list of elements of any data type.

    **Examples**: ``[0,1,2,3]``, ``[a,b,c]``, ``[true,false,true]``

.. data:: dict

    A 'dictionary'a, mapping key strings to values of any data type.
    
    A dictionary is created by enclosing one or more key-value pairs (a string followed by a colon and any JME expression) in square brackets, or with the ``dict`` function.

    Key strings are case-sensitive.

    **Examples**: 
    
    * ``["a": 1, "b": 2]``
    * ``["name": "Tess Tuser", "age": 106, "hobbies": ["reading","writing","arithmetic"] ]``
    * ``dict("key1": 0.1, "key2": 1..3)``
    * ``dict([["key1",1], ["key2",2]])``

    .. warning::
        Because lists and dicts use similar syntax, ``[]`` produces an empty list, **not** an empty dictionary. 
        To create an empty dictionary, use ``dict()``.

.. data:: range

    A range ``a..b#c`` represents (roughly) the set of numbers :math:`\{a+nc \: | \: 0 \leq n \leq \frac{b-a}{c} \}`. If the step size is zero, then the range is the continuous interval :math:`[a,b]`.

    **Examples**: ``1..3``, ``1..3#0.1``, ``1..3#0``

.. data:: set

    An unordered set of elements of any data type. The elements are pairwise distinct - if you create a set from a list with duplicate elements, the resulting set will not contain the duplicates. 

    **Examples**: ``set(a,b,c)``, ``set([1,2,3,4])``, ``set(1..5)``

.. data:: vector

    The components of a vector must be numbers.

    When combining vectors of different dimensions, the smaller vector is padded with zeros to make up the difference.

    **Examples**: ``vector(1,2)``, ``vector([1,2,3,4])``

.. data:: matrix

    Matrices are constructed from lists of numbers, representing the rows.

    When combining matrices of different dimensions, the smaller matrix is padded with zeros to make up the difference.
    
    **Examples**: ``matrix([1,2,3],[4,5,6])``, ``matrix(row1,row2,row3)``

.. data:: html

    An HTML DOM node.

    **Examples**: ``html("<div>things</div>")``

Function reference
******************

Arithmetic
----------

.. function:: x+y

    Addition. Numbers, vectors, matrices, lists, dicts, or strings can be added together.
    
    * ``list1+list2`` concatenates the two lists, while ``list+value`` returns a list with the right-hand-side value appended.
    * ``dict1+dict2`` merges the two dictionaries, with values from the right-hand side taking precedence when the same key is present in both dictionaries.

    **Examples**: 
        * ``1+2`` → ``3``
        * ``vector(1,2)+vector(3,4)`` → ``vector(4,6)``
        * ``matrix([1,2],[3,4])+matrix([5,6],[7,8])`` → ``matrix([6,8],[10,12])``
        * ``[1,2,3]+4`` → ``[1,2,3,4]``
        * ``[1,2,3]+[4,5,6]`` → ``[1,2,3,4,5,6]``
        * ``"hi "+"there"`` → ``"hi there"``

.. function:: x-y

    Subtraction. Defined for numbers, vectors and matrices.

    **Examples**: 
        * ``1-2`` → ``-1``
        * ``vector(3,2)-vector(1,4)`` → ``vector(2,-2)``
        * ``matrix([5,6],[3,4])-matrix([1,2],[7,8])`` → ``matrix([4,4],[-4,-4])``

.. function:: x*y

    Multiplication. Numbers, vectors and matrices can be multiplied together.

    **Examples**: 
        * ``1*2`` → ``2``
        * ``2*vector(1,2,3)`` → ``vector(2,4,6)``
        * ``matrix([1,2],[3,4])*2`` → ``matrix([2,4],[6,8])``
        * ``matrix([1,2],[3,4])*vector(1,2)`` → ``vector(5,11)``

.. function:: x/y

    Division. Only defined for numbers. 

    **Example**: ``3/4`` → ``0.75``.

.. function:: x^y

    Exponentiation. Only defined for numbers.

    **Examples**: 
        * ``3^2`` → ``9``
        * ``exp(3,2)`` → ``9``
        * ``e^(pi * i)`` → ``-1``

Number operations
-----------------

.. function:: abs(x)
              len(x)
              length(x)

    Absolute value, or modulus. 
    Defined for numbers, strings, ranges, vectors, lists and dictionaries.
    In the case of a list, returns the number of elements. 
    For a range, returns the difference between the upper and lower bounds.
    For a dictionary, returns the number of keys.

    **Examples**: 
        * ``abs(-8)`` → ``8``
        * ``abs(3-4i)`` → ``5``
        * ``abs("Hello")`` → ``5``
        * ``abs([1,2,3])`` → ``3``
        * ``len([1,2,3])`` → ``3``
        * ``len(set([1,2,2]))`` → ``2``
        * ``length(vector(3,4))`` → ``5``
        * ``abs(vector(3,4,12))`` → ``13``
        * ``len(["a": 1, "b": 2, "c": 1])`` → ``3``

.. function:: arg(z)

    Argument of a complex number.

    **Example**: ``arg(-1)`` → ``pi``

.. function:: re(z)

    Real part of a complex number.

    **Example**: ``re(1+2i)`` → ``1``

.. function:: im(z)

    Imaginary part of a complex number.

    **Example**: ``im(1+2i)`` → ``2``

.. function:: conj(z)

    Complex conjugate.

    **Example**: ``conj(1+i)`` → ``1-i``

.. function:: isint(x)

    Returns ``true`` if ``x`` is an integer.

    **Example**: ``isint(4.0)`` → ``true``

.. function:: sqrt(x)
              sqr(x)

    Square root of a number.

    **Examples**: 
        * ``sqrt(4)`` → ``2``
        * ``sqrt(-1)`` → ``i``

.. function:: root(x,n)

    ``n``:sup:`th` root of ``x``.

    **Example**: ``root(8,3)`` → ``2``.

.. function:: ln(x)

    Natural logarithm.

    **Example**: ``ln(e)`` → ``1``

.. function:: log(x)

    Logarithm with base 10.

    **Example**: ``log(100)`` → ``2``.

.. function:: log(x,b)

    Logarithm with base ``b``.

    **Example**: ``log(8,2)`` → ``3``.

.. function:: degrees(x)

    Convert radians to degrees.

    **Examples**: ``degrees(pi/2)`` → ``90``

.. function:: radians(x)

    Convert degrees to radians.

    **Examples**: ``radians(180)`` → ``pi``

.. function:: sign(x)
              sgn(x)

    Sign of a number. Equivalent to :math:`\frac{x}{|x|}`, or 0 when ``x`` is 0.

    **Examples**: 
        * ``sign(3)`` → ``1``
        * ``sign(-3)`` → ``-1``

.. function:: max(a,b)

    Greatest of two numbers.

    **Example**: ``max(46,2)`` → ``46``

.. function:: max(list)

    Greatest of a list of numbers.

    **Example**: ``max([1,2,3])`` → ``3``

.. function:: min(a,b)

    Least of two numbers.

    **Example**: ``min(3,2)`` → ``2``

.. function:: min(list)

    Least of a list of numbers.

    **Example**: ``min([1,2,3])`` → ``1``

.. function:: precround(n,d)

    Round ``n`` to ``d`` decimal places.
    On matrices and vectors, this rounds each element independently.

    **Examples**: 
        * ``precround(pi,5)`` → ``3.14159``
        * ``precround(matrix([[0.123,4.56],[54,98.765]]),2)`` → ``matrix([[0.12,4.56],[54,98.77]])``
        * ``precround(vector(1/3,2/3),1)`` → ``vector(0.3,0.7)``

.. function:: siground(n,f)

    Round ``n`` to ``f`` significant figures.
    On matrices and vectors, this rounds each element independently.

    **Examples**: 
        * ``siground(pi,3)`` → ``3.14``
        * ``siground(matrix([[0.123,4.56],[54,98.765]]),2)`` → ``matrix([[0.12,4.6],[54,99]])``
        * ``siground(vector(10/3,20/3),2)`` → ``vector(3.3,6.7)``

.. function:: dpformat(n,d,[style])

    Round ``n`` to ``d`` decimal places and return a string, padding with zeros if necessary.
    
    If ``style`` is given, the number is rendered using the given notation style.
    See the page on :ref:`number-notation` for more on notation styles.

    **Example**: ``dpformat(1.2,4)`` → ``"1.2000"``

.. function:: sigformat(n,d,[style])

    Round ``n`` to ``d`` significant figures and return a string, padding with zeros if necessary.

    **Example**: ``sigformat(4,3)`` → ``"4.00"``

.. function:: formatnumber(n,style)

    Render the number ``n`` using the given number notation style.

    See the page on :ref:`number-notation` for more on notation styles.

    **Example**: ``formatnumber(1234.567,"fr")`` → ``"1.234,567"``

.. function:: string(n)

    Render the number ``n`` using the ``plain-en`` notation style.

.. function:: parsenumber(string,style)

    Parse a string representing a number written in the given style.

    See the page on :ref:`number-notation` for more on notation styles.

    **Example**: ``parsenumber("1 234,567","si-fr")`` → ``1234.567``

Trigonometry
------------

Trigonometric functions all work in radians, and have as their domain the complex numbers.

.. function:: sin(x)

    Sine.

.. function:: cos(x)

    Cosine.

.. function:: tan(x)

    Tangent: :math:`\tan(x) = \frac{\sin(x)}{\cos(x)}`

.. function:: cosec(x)

    Cosecant: :math:`\csc(x) = \frac{1}{sin(x)}`

.. function:: sec(x)

    Secant: :math:`\sec(x) = \frac{1}{cos(x)}`

.. function:: cot(x)

    Cotangent: :math:`\cot(x) = \frac{1}{\tan(x)}`

.. function:: arcsin(x)

    Inverse of :func:`sin`. When :math:`x \in [-1,1]`, ``arcsin(x)`` returns a value in :math:`[-\frac{\pi}{2}, \frac{\pi}{2}]`.

.. function:: arccos(x)

    Inverse of :func:`cos`. When :math:`x \in [-1,1]`, ``arccos(x)`` returns a value in :math:`[0, \frac{\pi}]`.

.. function:: arctan(x)

    Inverse of :func:`tan`. When :math:`x` is non-complex, ``arctan(x)`` returns a value in :math:`[-\frac{\pi}{2}, \frac{\pi}{2}]`.

.. function:: sinh(x)

    Hyperbolic sine: :math:`\sinh(x) = \frac{1}{2} \left( \mathrm{e}^x - \mathrm{e}^{-x} \right)`

.. function:: cosh(x)
    
    Hyperbolic cosine: :math:`\cosh(x) = \frac{1}{2} \left( \mathrm{e}^x + \mathrm{e}^{-x} \right)`

.. function:: tanh(x)

    Hyperbolic tangent: :math:`tanh(x) = \frac{\sinh(x)}{\cosh(x)}`

.. function:: cosech(x)

    Hyperbolic cosecant: :math:`\operatorname{cosech}(x) = \frac{1}{\sinh(x)}`

.. function:: sech(x)

    Hyperbolic secant: :math:`\operatorname{sech}(x) = \frac{1}{\cosh(x)}`

.. function:: coth(x)

    Hyperbolic cotangent: :math:`\coth(x) = \frac{1}{\tanh(x)}`

.. function:: arcsinh(x)

    Inverse of :func:`sinh`.

.. function:: arccosh(x)

    Inverse of :func:`cosh`.

.. function:: arctanh(x)

    Inverse of :func:`tanh`.

Number theory
-------------

.. function:: x!

    Factorial. When ``x`` is not an integer, :math:`\Gamma(x+1)` is used instead.

    **Examples**: 
        * ``fact(3)`` → ``6``
        * ``3!`` → ``6``
        * ``fact(5.5)`` → ``287.885277815``

.. function:: factorise(n)

    Factorise ``n``. Returns the exponents of the prime factorisation of ``n`` as a list.

    **Examples**
        * ``factorise(18)`` → ``[1,2]``
        * ``factorise(70)`` → ``[1,0,1,1]``

.. function:: gamma(x)

    Gamma function.

    **Examples**: 
        * ``gamma(3)`` → ``2``
        * ``gamma(1+i)`` → ``0.4980156681 - 0.1549498283i``

.. function:: ceil(x)

    Round up to the nearest integer. When ``x`` is complex, each component is rounded separately.

    **Examples**: 
        * ``ceil(3.2)`` → ``4``
        * ``ceil(-1.3+5.4i)`` → ``-1+6i``

.. function:: floor(x)

    Round down to the nearest integer. When ``x`` is complex, each component is rounded separately.

    **Example**: ``floor(3.5)`` → ``3``

.. function:: trunc(x)

    If ``x`` is positive, round down to the nearest integer; if it is negative, round up to the nearest integer.

    **Example**: 
        * ``trunc(3.3)`` → ``3``
        * ``trunc(-3.3)`` → ``-3``

.. function:: fract(x)

    Fractional part of a number. Equivalent to ``x-trunc(x)``.

    **Example**: ``fract(4.3)`` → ``0.3``

.. function:: rational_approximation(n,[accuracy])

    Compute a rational approximation to the given number by computing terms of its continued fraction, returning the numerator and denominator separately.
    The approximation will be within :math:`e^{-\text{accuracy}}` of the true value; the default value for ``accuracy`` is 15.

    **Examples**: 
        * ``rational_approximation(pi)`` → ``[355,113]``
        * ``rational_approximation(pi,3)`` → ``[22,7]``

.. function:: mod(a,b)

    Modulo; remainder after integral division, i.e. :math:`a \bmod b`.

    **Example**: ``mod(5,3)`` → ``2``

.. function:: perm(n,k)

    Count permutations, i.e. :math:`^n \kern-2pt P_k = \frac{n!}{(n-k)!}`.

    **Example**: ``perm(5,2)`` → ``20``

.. function:: comb(n,k)

    Count combinations, i.e. :math:`^n \kern-2pt C_k = \frac{n!}{k!(n-k)!}`.

    **Example**: ``comb(5,2)`` → ``10``.

.. function:: gcd(a,b)
              gcf(a,b)

    Greatest common divisor of integers ``a`` and ``b``. Can also write ``gcf(a,b)``.

    **Example**: ``gcd(12,16)`` → ``4``

.. function:: lcm(a,b)

    Lowest common multiple of integers ``a`` and ``b``. Can be used with any number of arguments; it returns the lowest common multiple of all the arguments.

    **Examples** 
        * ``lcm(8,12)`` → ``24``
        * ``lcm(8,12,5)`` → ``120``

.. function:: x|y

    ``x`` divides ``y``.

    **Example**: ``4|8`` → ``true``

Vector arithmetic
-----------------

.. function:: vector(a1,a2,...,aN)

    Create a vector with given components. Alternately, you can create a vector from a single list of numbers.

    **Examples**:
        * ``vector(1,2,3)``
        * ``vector([1,2,3])``

.. function:: matrix(row1,row2,...,rowN)

    Create a matrix with given rows, which should be lists of numbers. Or, you can pass in a single list of lists of numbers.

    **Examples**: 
        * ``matrix([1,2],[3,4])``
        * ``matrix([[1,2],[3,4]])``

.. function:: rowvector(a1,a2,...,aN)

    Create a row vector (:math:`1 \times n` matrix) with the given components. Alternately, you can create a row vector from a single list of numbers.

    **Examples**: 
        * ``rowvector(1,2)`` → ``matrix([1,2])``
        * ``rowvector([1,2])`` → ``matrix([1,2])``

.. function:: dot(x,y)

    Dot (scalar) product. Inputs can be vectors or column matrices.

    **Examples**: ``dot(vector(1,2,3),vector(4,5,6))``, ``dot(matrix([1],[2]), matrix([3],[4])``.

.. function:: cross(x,y)

    Cross product. Inputs can be vectors or column matrices.

    **Examples**: ``cross(vector(1,2,3),vector(4,5,6))``, ``cross(matrix([1],[2]), matrix([3],[4])``.

.. function:: angle(a,b)
    
    Angle between vectors ``a`` and ``b``, in radians.
    Returns ``0`` if either ``a`` or ``b`` has length 0.

    **Example**: ``angle(vector(1,0),vector(0,1))``

.. function:: det(x)

    Determinant of a matrix. Only defined for up to 3x3 matrices.

    **Examples**: ``det(matrix([1,2],[3,4]))``, ``det(matrix([1,2,3],[4,5,6],[7,8,9]))``.

.. function:: transpose(x)
    
    Matrix transpose. Can also take a vector, in which case it returns a single-row matrix.

    **Examples**: ``transpose(matrix([1,2],[3,4]))``, ``transpose(vector(1,2,3))``.

.. function:: id(n)

    Identity matrix with :math:`n` rows and columns.

    **Example**: ``id(3)``.

Strings
------------------

.. function:: x[n]

    Get the Nth character of the string ``x``.
    Indices start at 0.

    **Example**: ``"hello"[1]`` → ``"e"``

.. function:: x[a..b]

    Slice the string ``x`` - get the substring between the given indices.
    Note that indices start at 0, and the final index is not included.

    **Example**: ``"hello"[1..4]`` → ``"ell"``

.. function:: substring in string

    Test if ``substring`` occurs anywhere in ``string``.
    This is case-sensitive.

    **Example**: ``"plain" in "explains"`` → ``true``

.. function:: latex(x)

    Mark string ``x`` as containing raw LaTeX, so when it's included in a mathmode environment it doesn't get wrapped in a ``\textrm`` environment.
    
    Note that backslashes must be double up, because the backslash is an escape character in JME strings.

    **Example**: ``latex('\\frac{1}{2}')``.

.. function:: safe(x)

    Mark string ``x`` as safe: don't substitute variable values into it when this expression is evaluated.

    Use this function to preserve curly braces in string literals.

    **Example**: ``safe('From { to }')``

.. function:: capitalise(x)

    Capitalise the first letter of a string.

    **Example**: ``capitalise('hello there')``.

.. function:: pluralise(n,singular,plural)

    Return ``singular`` if ``n`` is 1, otherwise return ``plural``.

    **Example**: ``pluralise(num_things,"thing","things")``

.. function:: upper(x)

    Convert string to upper-case.

    **Example**: ``upper('Hello there')``.

.. function:: lower(x)

    Convert string to lower-case.

    **Example**: ``lower('CLAUS, Santa')``.

.. function:: join(strings, delimiter)

    Join a list of strings with the given delimiter.

    **Example**: ``join(['a','b','c'],',')`` → ``'a,b,c'``

.. function:: split(string,delimiter)

    Split a string at every occurrence of ``delimiter``, returning a list of the the remaining pieces.

    **Example**: ``split("a,b,c,d",",")`` → ``["a","b","c","d"]``

.. function:: currency(n,prefix,suffix)

    Write a currency amount, with the given prefix or suffix characters.

    **Example**: ``currency(123.321,"£","")`` → ``'£123.32'``

.. function:: separateThousands(n,separator)

    Write a number, with the given separator character between every 3 digits

    To write a number using notation appropriate to a particular culture or context, see :func:`formatnumber`.

    **Example**: ``separateThousands(1234567.1234,",")`` → ``'1,234,567.1234'``

.. function:: lpad(str, n, prefix)

    Add copies of ``prefix`` to the start of ``str`` until the result is at least ``n`` characters long.

    **Example**: ``lpad("3", 2, "0")`` → ``"03"``

.. function:: rpad(str, n, suffix)

    Add copies of ``suffix`` to the end of ``str`` until the result is at least ``n`` characters long.

    **Example**: ``rpad("3", 2, "0")`` → ``"30"``

Logic
-----

.. function:: x<y

    Returns ``true`` if ``x`` is less than ``y``. Defined only for numbers.

    **Examples**: ``4<5``.

.. function:: x>y

    Returns ``true`` if ``x`` is greater than ``y``. Defined only for numbers.

    **Examples**: ``5>4``.

.. function:: x<=y

    Returns ``true`` if ``x`` is less than or equal to ``y``. Defined only for numbers.

    **Examples**: ``4<=4``.

.. function:: x>=y

    Returns ``true`` if ``x`` is greater than or equal to ``y``. Defined only for numbers.

    **Examples**: ``4>=4``.

.. function:: x<>y

    Returns ``true`` if ``x`` is not equal to ``y``. Defined for any data type. Returns ``true`` if ``x`` and ``y`` are not of the same data type.

    **Examples**: ``'this string' <> 'that string'``, ``1<>2``, ``'1' <> 1``.

.. function:: x=y

    Returns ``true`` if ``x`` is equal to ``y``. Defined for any data type. Returns ``false`` if ``x`` and ``y`` are not of the same data type.

    **Examples**: ``vector(1,2)=vector(1,2,0)``, ``4.0=4``.

.. function:: x and y

    Logical AND. Returns ``true`` if both ``x`` and ``y`` are true, otherwise returns false.

    **Examples**: ``true and true``, ``true && true``, ``true & true``.

.. function:: not x

    Logical NOT. 

    **Examples**: ``not true``, ``!true``.

.. function:: x or y

    Logical OR. Returns ``true`` when at least one of ``x`` and ``y`` is true. Returns false when both ``x`` and ``y`` are false.

    **Examples**: ``true or false``, ``true || false``.

.. function:: x xor y

    Logical XOR. Returns ``true`` when at either ``x`` or ``y`` is true but not both. Returns false when ``x`` and ``y`` are the same expression.

    **Example**: ``true XOR false``.

.. function:: x implies y

    Logical implication. If ``x`` is true and ``y`` is false, then the implication is false. Otherwise, the implication is true.
    
    **Example**: ``false implies true``.

Ranges
------

.. function:: a..b

    Define a range. Includes all integers between and including ``a`` and ``b``.

    **Examples**: ``1..5``, ``-6..6``.

.. function:: a..b#s

    Set the step size for a range. Default is 1. When ``s`` is 0, the range includes all real numbers between the limits.

    **Examples**: ``0..1 # 0.1``, ``2..10 # 2``, ``0..1#0``.

.. function:: a except b

    Exclude a number, range, or list of items from a list or range.

    **Examples**: ``-9..9 except 0``, ``-9..9 except [-1,1]``. ``3..8 except 4..6``, ``[1,2,3,4,5] except [2,3]``.

.. function:: list(range)

    Convert a range to a list of its elements.

    **Example**: ``list(-2..2)`` → ``[-2,-1,0,1,2]``

Lists
-----

.. function:: x[n]

    Get the ``n``:sup:`th` element of list, vector or matrix ``x``. For matrices, the ``n``:sup:`th` row is returned.

    **Example**: 
        * ``[0,1,2,3][1]`` → ``1``
        * ``vector(0,1,2)[2]`` → ``2``
        * ``matrix([0,1,2],[3,4,5],[6,7,8])[0]`` → ``matrix([0,1,2])``

.. function:: x[a..b]

    Slice list ``x`` - return elements with indices in the given range.
    Note that list indices start at 0, and the final index is not included.

    **Example**: ``[0,1,2,3,4,5][1..3]`` → ``[1,2]``

.. function:: x in collection

    Is element ``x`` in the list, set or range ``collection``?

    If ``collection`` is a dictionary, returns ``true`` if the dictionary has a key ``x``.

    **Examples**: 
        * ``3 in [1,2,3,4]`` → ``true``
        * ``3 in (set(1,2,3,4) and set(2,4,6,8))`` → ``false``
        * ``"a" in ["a": 1]`` → ``true``
        * ``1 in ["a": 1]`` throws an error because dictionary keys must be strings.

.. function:: repeat(expression,n)

    Evaluate ``expression`` ``n`` times, and return the results in a list.

    **Example**: ``repeat(random(1..4),5)`` → ``[2, 4, 1, 3, 4]``

.. function:: map(expression,name[s],d)

    Evaluate ``expression`` for each item in list, range, vector or matrix ``d``, replacing variable ``name`` with the element from ``d`` each time.

    You can also give a list of names if each element of ``d`` is a list of values. 
    The Nth element of the list will be mapped to the Nth name.

    .. note::
        Do not use ``i`` or ``e`` as the variable name to map over - they're already defined as mathematical constants!

    **Examples**: 
        * ``map(x+1,x,1..3)`` → ``[2,3,4]``
        * ``map(capitalise(s),s,["jim","bob"])`` → ``["Jim","Bob"]``
        * ``map(sqrt(x^2+y^2),[x,y],[ [3,4], [5,12] ])`` → ``[5,13]``
        * ``map(x+1,x,id(2))`` → ``matrix([[2,1],[1,2]])``
        * ``map(sqrt(x),x,vector(1,4,9))`` → ``vector(1,2,3)``

.. function:: filter(expression,name,d)

    Filter each item in list or range ``d``, replacing variable ``name`` with the element from ``d`` each time, returning only the elements for which ``expression`` evaluates to ``true``.

    .. note::
        Do not use ``i`` or ``e`` as the variable name to map over - they're already defined as mathematical constants!

    **Example**: ``filter(x>5,x,[1,3,5,7,9])`` → ``[7,9]``

.. function:: let(name,definition,...,expression)
              let(definitions, expression)

    Evaluate ``expression``, temporarily defining variables with the given names. 
    Use this to cut down on repetition. 
    You can define any number of variables - in the first calling pattern, follow a variable name with its definition.
    Or you can give a dictionary mapping variable names to their values.
    The last argument is the expression to be evaluated.

    **Examples**: 
        * ``let(d,sqrt(b^2-4*a*ac), [(-b+d)/2, (-b-d)/2])`` → ``[-2,-3]`` (when ``[a,b,c]`` = ``[1,5,6]``)
        * ``let(x,1, y,2, x+y)`` → ``3``
        * ``let(["x": 1, "y": 2], x+y)`` → ``3``

.. function:: sort(x)

    Sort list ``x``.

    **Example**: ``sort([4,2,1,3])`` → ``[1,2,3,4]``

.. function:: reverse(x)

    Reverse list ``x``.

    **Example**: ``reverse([1,2,3])`` → ``[3,2,1]``

.. function:: indices(list,value)

    Find the indices at which ``value`` occurs in ``list``.

    **Examples**:
        * ``indices([1,0,1,0],1)`` → ``[0,2]``
        * ``indices([2,4,6],4)`` → ``[1]``
        * ``indices([1,2,3],5)`` → ``[]``

.. function:: distinct(x)

    Return a copy of the list ``x`` with duplicates removed.

    **Example**: ``distinct([1,2,3,1,4,3])`` → ``[1,2,3,4]``

.. function:: list(x)

    Convert set, vector or matrix ``x`` to a list of components (or rows, for a matrix).

    **Examples**: 
        * ``list(set(1,2,3))`` → ``[1,2,3]`` (note that you can't depend on the elements of sets being in any order)
        * ``list(vector(1,2))`` → ``[1,2]``
        * ``list(matrix([1,2],[3,4]))`` → ``[[1,2], [3,4]]``

.. function:: satisfy(names,definitions,conditions,maxRuns)

    Each variable name in ``names`` should have a corresponding definition expression in ``definitions``. ``conditions`` is a list of expressions which you want to evaluate to ``true``. The definitions will be evaluated repeatedly until all the conditions are satisfied, or the number of attempts is greater than ``maxRuns``. If ``maxRuns`` isn't given, it defaults to 100 attempts.

    **Example**: ``satisfy([a,b,c],[random(1..10),random(1..10),random(1..10)],[b^2-4*a*c>0])``

.. function:: sum(numbers)

    Add up a list of numbers

    **Example**: ``sum([1,2,3])`` → ``6``

.. function:: product(list1,list2,...,listN)

    Cartesian product of lists. In other words, every possible combination of choices of one value from each given list.

    **Example**: ``product([1,2],[a,b])`` → ``[ [1,a], [1,b], [2,a], [2,b] ]``

.. function:: zip(list1,list2,...,listN)

    Combine two (or more) lists into one - the Nth element of the output is a list containing the Nth elements of each of the input lists.

    **Example**: ``zip([1,2,3],[4,5,6])`` → ``[ [1,4], [2,5], [3,6] ]``

.. function:: combinations(collection,r)

    All ordered choices of ``r`` elements from ``collection``, without replacement.

    **Example**: ``combinations([1,2,3],2)`` → ``[ [1,2], [1,3], [2,3] ]``

.. function:: combinations_with_replacement(collection,r)

    All ordered choices of ``r`` elements from ``collection``, with replacement.

    **Example**: ``combinations([1,2,3],2)`` → ``[ [1,1], [1,2], [1,3], [2,2], [2,3], [3,3] ]``

.. function:: permutations(collection,r)

    All choices of ``r`` elements from ``collection``, in any order, without replacement.

    **Example**: ``permutations([1,2,3],2)`` → ``[ [1,2], [1,3], [2,1], [2,3], [3,1], [3,2] ]``

Dictionaries
------------

.. function:: dict[key]

    Get the value corresponding to the given key string in the dictionary ``d``.

    If the key is not present in the dictionary, an error will be thrown.

    **Example**: ``["a": 1, "b": 2]["a"]`` → ``1``

.. function:: get(dict,key,default)

    Get the value corresponding to the given key string in the dictionary.

    If the key is not present in the dictionary, the ``default`` value will be returned.

    **Examples**:
        * ``get(["a":1], "a", 0)`` → ``1``
        * ``get(["a":1], "b", 0)`` → ``0``

.. function:: dict(keys)

    Create a dictionary with the given key-value pairs.
    Equivalent to ``[ .. ]``, except when no key-value pairs are given: ``[]`` creates an empty *list* instead.

    **Examples**:
        * ``dict()``
        * ``dict("a": 1, "b": 2)``

.. function:: keys(dict)

    A list of all of the given dictionary's keys.

    **Example**: ``keys(["a": 1, "b": 2, "c": 1])`` → ``["a","b","c"]``

.. function:: values(dict)
              values(dict,keys)

    A list of the values corresponding to each of the given dictionary's keys.

    If a list of keys is given, the values corresponding to those keys are returned, in the same order.

    **Examples**: 
        * ``values(["a": 1, "b": 2, "c": 1])`` → ``[1,2,1]``
        * ``values(["a": 1, "b": 2, "c": 3], ["b","a"])`` → ``[2,1]``

.. function:: items(dict)

    A list of all of the ``[key,value]`` pairs in the given dictionary.

    **Example**: ``values(["a": 1, "b": 2, "c": 1])`` → ``[ ["a",1], ["b",2], ["c",1] ]``

Sets
----

.. function:: set(a,b,c,...) or set([elements])

    Create a set with the given elements. Either pass the elements as individual arguments, or as a list.

    **Examples**: ``set(1,2,3)``, ``set([1,2,3])``

.. function:: union(a,b)

    Union of sets ``a`` and ``b``

    **Examples**:
        * ``union(set(1,2,3),set(2,4,6))`` → ``set(1,2,3,4,6)``
        * ``set(1,2,3) or set(2,4,6)`` → ``set(1,2,3,4,6)``

.. function:: intersection(a,b)

    Intersection of sets ``a`` and ``b``, i.e. elements which are in both sets

    **Examples**:
        * ``intersection(set(1,2,3),set(2,4,6))`` → ``set(2)``
        * ``set(1,2,3) and set(2,4,6)`` → ``set(2)``

.. function:: a-b

    Set minus - elements which are in a but not b

    **Example**: ``set(1,2,3,4) - set(2,4,6)`` → ``set(1,3)``

Randomisation
-------------

.. function:: random(x)

    Pick uniformly at random from a range, list, or from the given arguments.

    **Examples**: 
        * ``random(1..5)``
        * ``random([1,2,4])``
        * ``random(1,2,3)``

.. function:: deal(n)

    Get a random shuffling of the integers :math:`[0 \dots n-1]`

    **Example**: ``deal(3)`` → ``[2,0,1]``

.. function:: shuffle(x) or shuffle(a..b)

    Random shuffling of list or range.

    **Examples**: 
        * ``shuffle(["a","b","c"])`` → ``["c","b","a"]``
        * ``shuffle(0..4)`` → ``[2,3,0,4,1]``

Control flow
------------

.. function:: award(a,b)

    Return ``a`` if ``b`` is ``true``, else return ``0``.

    **Example**: ``award(5,true)`` → ``5``

.. function:: if(p,a,b)

    If ``p`` is ``true``, return ``a``, else return ``b``. Only the returned value is evaluated.

    **Example**: ``if(false,1,0)`` → ``0``

.. function:: switch(p1,a1,p2,a2, ..., pn,an,d)

    Select cases. Alternating boolean expressions with values to return, with the final argument representing the default case. Only the returned value is evaluated.

    **Examples**: 
        * ``switch(true,1,false,0,3)`` → ``1``
        * ``switch(false,1,true,0,3)`` → ``0``
        * ``switch(false,1,false,0,3)`` → ``3``

HTML
----

.. function:: html(x)

    Parse string ``x`` as HTML.

    **Examples**: ``html('<div>Text!</div>')``.

.. function:: table(data), table(data,headers)

    Create an HTML with cell contents defined by ``data``, which should be a list of lists of data, and column headers defined by the list of strings ``headers``.

    **Examples**: 
        * ``table([[0,1],[1,0]], ["Column A","Column B"])``
        * ``table([[0,1],[1,0]])``

.. function:: image(url)

    Create an HTML `img` element loading the image from the given URL. Images uploaded through the resources tab are stored in the relative URL `resources/images/<filename>.png`, where `<filename>` is the name of the original file.

    **Examples**: 
        * ``image('resources/images/picture.png')``
        * ``image(chosenimage)``
        * `Question using randomly chosen images <https://numbas.mathcentre.ac.uk/question/1132/using-a-randomly-chosen-image/>`_.

JSON
----

`JSON <http://www.json.org/>`_ is a lightweight data-interchange format.
Many public data sets come in JSON format; it's a good way of encoding data in a way that is easy for both humans and computers to read and write.

For an example of how you can use JSON data in a Numbas question, see the exam `Working with JSON data <https://numbas.mathcentre.ac.uk/exam/4684/working-with-json-data/>`_.

.. function:: json_decode(json)

    Decode a JSON string into JME data types. 

    JSON is decoded into numbers, strings, booleans, lists, or dictionaries. 
    To produce other data types, such as matrices or vectors, you will have to post-process the resulting data.

    .. warning::
        The JSON value ``null`` is silently converted to an empty string, because JME has no "null" data type.
        This may change in the future.

    **Example**: ``json_decode(' {"a": 1, "b": [2,true,"thing"]} ')`` → ``["a": 1, "b": [2,true,"thing"]]``

.. function:: json_encode(data)

    Convert the given object to a JSON string.

    Numbers, strings, booleans, lists, and dictionaries are converted in a straightforward manner.
    Other data types may behave unexpectedly.

    **Example**: ``json_encode([1,"a",true])`` → ``'[1,"a",true]'``

Sub-expressions
---------------

.. function:: expression(string)

    Parse a string as a JME expression. 
    The expression can be substituted into other expressions, such as the answer to a mathematical expression part, or the ``\simplify`` LaTeX command.

    **Example**: `A question using randomly chosen variable names <https://numbas.mathcentre.ac.uk/question/20358/randomise-variable-names-expression-version/>`_.
