The Definitive Guide To PHP's isset And empty  
---------------------------------------------

**Source: [http://kunststube.net/isset/](http://kunststube.net/isset/)**

 

PHP has two very similar functions that are essential to writing good
PHP applications, but whose purpose and exact function is rarely well
explained: isset and empty. The PHP manual itself doesn't have a simple
explanation that actually captures their essence and most posts written
around the web seem to be missing some detail or other as well. This
article attempts to fill that gap; and takes a broad sweep of related
topics to do so.

About PHP's Error Reporting
---------------------------

To explain what these functions are needed for to begin with, it's
necessary to talk about PHP's error reporting mechanism. PHP is an
interpreted language that lacks a compilation step separate from the
actual runtime.^[1](http://kunststube.net/isset/#fn:compilation)^ That
is, you don't typically compile PHP source code into an executable, then
run that executable. PHP code is simply executed straight from the
source and it either works or dies halfway through. The PHP interpreter
and runtime can only complain about errors while the code is in full
motion. Since there is no separate compilation step, certain types of
errors that could be caught by a compiler can only surface at runtime.

There is now the dilemma between letting programs crash completely for
every little mistake, or silently letting non-critical errors slip
through. PHP is choosing the middle ground: it triggers an error, which
by default simply causes a message to be output wherever it occurred.
These errors can have [different
types](http://php.net/manual/en/errorfunc.constants.php), ranging from a
relatively harmless E\_NOTICE to a program-halting fatal E\_ERROR. How
these errors are handled can be customized. For example, their output
can be turned off entirely or they can be redirected into a log file.
They could also be turned into full-blown exceptions, which in turn can
be handled by other error handling
mechanisms.^[2](http://kunststube.net/isset/#fn:exceptions)^

The PHP error reporting mechanism is **vital** in developing
applications. What kind of errors should be triggered and whether they
should be displayed or not can be configured in
the [php.ini file](http://php.net/manual/en/errorfunc.configuration.php#ini.error-reporting) with
theerror\_reporting and display\_errors directives, a setting that can
also be changed during runtime. The error\_reporting directive should be
set to the highest possible value during development, but turned down or
off on live production servers. Seeing errors as they are happening is
important during development (unless one likes to take stabs into the
dark). At the same time, errors in production can be a serious security
leak (not to mention a nuisance) and must not be output for every
visitor to see.

One of the things that triggers an error of the type E\_NOTICE is the
act of trying to use a variable that does not exist. This can happen due
to simple typos or logic errors and error reporting helps to catch such
errors quickly. But there's also the legitimate case of simply not
knowing whether a variable exists or not and needing to find out. If
this legitimate case would trigger an error every time, it'd be
impossible to efficiently tell real errors from non-critical errors.
Which is where isset and empty come into play.

Simple Definition
-----------------

Here are the two functions defined in a nutshell:

bool isset( mixed \$var [, mixed ... ])

Returns true if the variable exists and is not null. Does not trigger an
error if the variable does not exist.

Example:

    $foo = 'bar';
    var_dump(isset($foo));        -> true

    $baz = null;
    var_dump(isset($baz));        -> false

    var_dump(isset($undefined));  -> false

bool empty( mixed \$var )

Returns true if the variable does not exist or its value equals false in
a loose comparison. Does not trigger an error if the variable does not
exist.

Example:

    $foo = 'bar';
    var_dump(empty($foo));        -> false

    $baz = null;
    var_dump(empty($baz));        -> true

    var_dump(empty($undefined));  -> true

The important and remarkable thing about this behavior is that when
trying to pass non-existent variables to normal functions, an error is
triggered. PHP first tries to get the *value* of the variable, then pass
it into the function:

    var_dump($foobar);   -> Notice: Undefined variable: foobar
                            NULL

isset and empty are not actually regular functions but *language
constructs*. That means they're part of the PHP language itself, do not
play by the normal rules of functions and can hence get away with not
triggering an error for non-existent variables. This is important when
trying to figure out whether a variable exists or not. A typical case is
trying to find out whether a certain value is present in the URL's query
part:

    if ($_GET['var'] == 'foo')

If \$\_GET['var'] actually exists, that is, if the URL
contains ?var=foo, this will work just fine, regardless of the value
of var. The presence or absence of \$\_GET['var'] is entirely in the
hand of the user though. If the user does not include ?var=foo in the
URL, this program will trigger an error.

To avoid this, we need to use isset:

    if (isset($_GET['var']) && $_GET['var'] == 'foo')

isset guards our naïve use of the non-existent variable. If it is not
set, the code does not proceed to the actual comparison and no error is
triggered. isset itself does not trigger an error. In a
nutshell, isset performs a\$var !== null check in a way that does not
trigger an error.

isset can accept multiple arguments and only returns true if all of them
are *set*:

    if (isset($foo, $_GET['bar'], $array['baz'])) {
        // all needed values exist, do something with them
    }

### Null

When talking about isset one inevitably must also talk about null. Let's
come back for a second to the predicament of PHP trying to skip over
trivial errors without crashing a program.

    $foo = $_GET['var'];
    $bar = 'bar';
    if ($foo == $bar) {
        // do something
    }

How *should* PHP behave in the above program if \$\_GET['var'] did not
exist? Sure, it will throw a tantrum and trigger a notice, and that's
good and fine. But how should it treat \$foo afterwards? As some sort of
special outcast that doesn't have a value? Should it trigger an error
every time \$foo is used hence? The answer is simple: PHP assigns the
value null in place of the non-existent variable. null is a type unto
its own. nullis not a boolean, not an integer, not a string, not an
object. null is of type null which can only have one
value: null. null is used to *mean* the absence of a value, but null is
just a regular value in itself. nullloosely compares to false (null ==
false, but null !== false).

PHP has a weird relationship to the value null. I am not clear about the
historical development of it, but apparently it was
exclusively *supposed* to be used for "non-existent" variables. Somehow
it got promoted to a proper type of its own though. At the time of
writing the [manual even states](http://php.net/null):

> Casting a variable to null will remove the variable and unset its
> value.

Unfortunately that's not entirely correct. I may even go so far as to
call it nonsense (which in fact I did in a [bug
report](https://bugs.php.net/bug.php?id=55164)). Interestingly, the
"cast" of a value to null is done with the syntax (unset)\$var. There's
no(null)\$var, like there is for all other primitive types. This kind of
casting is also rather nonsensical. The following two expressions are
equivalent:

    $foo = (unset)$bar;
    $foo = null;

Using the (unset) cast neither changes the value of \$bar nor does it
unset \$bar. The only effect it has is the assignment of the
value null to \$foo. \$foo is not unset in any way either, it continues
to exist with the value null. To actually remove a variable, the
function unset() needs to be used:

    unset($foo);

The \$foo variable now indeed ceased to exist. Its value is gone, any
further attempt to work with it will trigger an error:

    $foo = 'bar';
    unset($foo);
    var_dump($foo);

This outputs:

    Notice: Undefined variable: foo on line 3
    NULL

PHP rightly complains that the variable \$foo does not exist and the
value of this non-existent variable is given as null. On the other hand:

    $foo = null;
    var_dump($foo);

simply outputs:

    NULL

The value null is used as the default value if there is no value, but a
variable that holds the value null is still a perfectly fine variable.
PHP itself is somewhat schizophrenic about null and still has what I
assume to be historical references to null having the same effect
as unset and connotation of a variable not existing.

This hopefully explains why isset is checking for null. This has the
side effect of making it impossible to distinguish between a variable
that does not exist and a variable that has the value null.

Empty
-----

The empty construct doesn't really have any caveats like isset does.
It's straight forward the same as \$var == false, without triggering an
error if the variable doesn't exist. [As the manual
says](http://php.net/empty), the following things are considered
"empty":

-   "" (an empty string)
-   0 (0 as an integer)
-   0.0 (0 as a float)
-   "0" (0 as a string)
-   null
-   false
-   array() (an empty array)
-   var \$var; (a variable declared, but without a value in a class)

And again, it really is simply the same as a [loose comparison
to false](http://php.net/manual/en/types.comparisons.php). That
makes empty nothing more and nothing less than a convenient shortcut
for !isset(\$var) || \$var == false. It's most often used in its negated
form !empty(\$var), which is equivalent to isset(\$var) && \$var ==
true. It's a great function to use if you expect a *truthy* value but
are not sure if the variable exists at all:

    if (empty($_GET['foo'])) {
        echo 'Error: please supply a valid value';
    }

    if (!empty($array['foo'])) {
        // array key exists and has a useful value, do something with it
    }

    if (!empty($foo) && is_array($foo)) {
        // $foo exists and is a non-empty array, do something with it
    }

Function Return Values
----------------------

Something that may cause some irritation is the fact that you can't
use isset and empty with functions:

    isset(myFunction())
    empty(myFunction())

    -> Fatal error: Can't use function return value in write context

This would be a valid expression with any other function,
like is\_numeric(myFunction()), since it simply passes the return value
of myFunction to the input of is\_numeric. isset and empty are language
constructs though and are meant to be used on variables only. Trying to
test whether the output of a functionisset or is empty results in the
above error. But it is never necessary to use the facilities
of isset orempty for this purpose anyway. A function *must* exist,
otherwise the program will crash with this message:

> Fatal error: Call to undefined function

This is the type of error PHP does *not* silently forgive and forget.
Furthermore, all functions always have a return value, at least an
implicit return value of null (again, the value for "no value"). Hence,
the following is the correct way of checking function return values:

    if (myFunction() !== null)  -> equivalent of isset($var)

    if (myFunction() == true)   -> equivalent of !empty($var)
    if (myFunction())           -> concise equivalent of !empty($var)

    if (myFunction() == false)  -> equivalent of empty($var)
    if (!myFunction())          -> concise equivalent of empty($var)

To check whether a function *exists*, i.e. *is declared*,
there's [function\_exists](http://www.php.net/function_exists).

**Update:** As of PHP 5.5 *expressions* are valid arguments
for empty and isset. I.e. empty(foo() == 'bar') and such are working
without error now. As explained above though, it hardly makes any sense
to use it, because you're just looking for a regular comparison
operation which doesn't require the special facilities ofisset or empty.

Application
-----------

The correct place to use isset and empty is in one and only one
situation: To test whether a variable that may or may not exist exists
(isset) and optionally whether it's also *falsey* (empty). The number
one use case is with GET or POST values:

    if (isset($_POST['username'], $_POST['password'])) {
        // login form was submitted correctly,
        // try to log the user in
    }

    // show login form

Any external user input is entirely beyond the control of the
programmer, hence these values may *legitimately not exist* and need to
be treated as such. By the way, GET and POST values can never
be null, isset's behavior regarding null values is therefore of no
concern.

If a variable *should* exist at some specific point in an application,
the use of isset and empty is **not recommended**.

    $foo = someComplicatedLogic();

    if (empty($foo)) {
        // someComplicatedLogic() returned something falsy
    }

The above is a *bad* use of empty. It may seem very expressive, since it
reads like *"if \$foo is empty..."*, but this is foregoing the
advantages of PHP's error reporting. Consider this:

    $somePrettyLongVariableName = someComplicatedLogic();

    if (empty($somePretyLongVariableName)) {
        // someComplicatedLogic() returned something falsy
    }

It's very easy to waste half a day and a lot of hair on the above code,
suspecting some logic error in thesomeComplicatedLogic() function,
debugging everything line by line, wondering why oh why the code reports
that someComplicatedLogic() returned something falsy when in fact
everything seems to be working fine. This should have been written as:

    $somePrettyLongVariableName = someComplicatedLogic();

    if (!$somePretyLongVariableName) {
        // someComplicatedLogic() returned something falsy
    }

The result would be the same, but here PHP would helpfully complain
with Notice: Undefined variable: somePretyLongVariableName on line 3.
The programmer would slap his forehead, fix the typo and move on with
life. Which serves as a good summary for this whole article:

-   always develop with error reporting [turned to
    11](http://en.wikipedia.org/wiki/Up_to_eleven) (and fix all real
    errors)
-   always use empty or isset for variables that may *legitimately* not
    exist
-   never use empty or isset for variables that *should* exist

The point of PHP's error reporting is to help the developer spot easy
mistakes which other languages would complain about at compile time. The
point of isset and empty is to specifically suppress Notice: Undefined
variable errors when the programmer couldn't otherwise avoid
it. empty is a shortcut forisset + boolean comparison.

And that's the whole story.

Epilog
------

### Edge Cases

If a page *requires* that, say, the query parameter foo is supplied in
the URL and *all* links to that page *should*include this parameter, I'd
err on the side of *not* using isset or empty. There's no way to make
sure the parameter is set in the URL, but if the application is designed
in a way that it *should* be set, it's not incorrect behavior to trigger
an error. It can help to quickly catch problems with invalid links
during development and since error reporting will be silenced in
production, it will not inconvenience any user. This is highly dependent
on how errors are handled in the application and how the rest of the
application flow goes though.

    // http://example.com/get_record.php?id=42

    $record = fetchRecordFromDatabase($_GET['id']);
    if (!$record) {
        errorPage(404);
    }

    echo $record;

This very succinctly handles any form of invalid URL with a 404 page and
even outputs helpful debug errors during development. Of course it makes
some assumptions of how the
examplefetchRecordFromDatabase and errorPage functions work. To handle
the case of a non-existing\$\_GET['id'] separately would require a lot
more code. Whether that's necessary needs to be judged for every case
individually.

### Array Keys

Specifically for arrays, there's an alternative
to isset(\$array['key']) called [array\_key\_exists](http://php.net/array_key_exists).
This function specifically does what it says: it returns true if an
array key *exists*, regardless of the value of that key. That makes it
possible to detect null values in arrays:

    $array = array('key' => null);

    var_dump(isset($array['key']));              -> false
    var_dump(array_key_exists('key', $array));   -> true

This *may* be useful in some cases, but is in my opinion rarely
necessary. null means "no value". Variables are just the things that
give the programmer a handle on values. A non-existent variable and no
value are essentially the same thing, hence the behavior of isset should
be sufficient in most cases. A legitimate exception is when you're
explictly working with language primitives, for example working on a
JSON encoder that needs to translate the PHP null value into
a 'null' JSON value. Most business logic code though won't need this.

### Initializing Variables

After all this talk about isset and empty, it's time to mention that
it's really not necessary to use them often. Any proper application
should *initialize* its variables, which makes checking for the
existence of variables a rare occurrence.

#### Regular Code Blocks

Any logical block of code should initialize the variables it's going to
work on beforehand:

    $foo = 'bar';
    $baz = null;

    // do complicated things

There's no need to check whether \$foo or \$baz exist after this point,
because they do. This serves as self-documentation, makes sure variables
always have a known default value and most of all helps to catch typos,
because PHP can now properly complain about non-existent variables.

#### Functions

Any arguments defined in a function declaration exist as variables
inside the function.

    function foo($bar, $baz = null) { ... }

There's no need to check whether \$bar or \$baz *exist* inside the
function body, because they do. Even if no value was passed for the
second \$baz parameter, the variable \$baz does in fact exist and has
the default value null.

#### Arrays

Arrays can be initialized to hold default values easily using the array
union operator + or array\_merge:

    function foo(array $options) {
        $options += array('bar' => true, 'baz' => false);
        ...
    }

There's no need to check whether \$options contains the
keys 'bar' or 'baz', because it does. And they even have known, good
default values.

* * * * *

1.  To be exact, there *are* compilers for PHP, but by far the majority
    of PHP applications are not separately
    compiled. [↩](http://kunststube.net/isset/#fnref:compilation)

2.  Please note that errors are not exceptions. These are two
    independent error reporting
    mechanisms. [↩](http://kunststube.net/isset/#fnref:exceptions)

About The Author
----------------

David C. Zentgraf is a web developer working partly in Japan and Europe
and is [a regular](http://stackoverflow.com/users/476/deceze) on [Stack
Overflow](http://stackoverflow.com/). If you have feedback, criticism or
additions, please feel free to
try [@deceze](http://twitter.com/deceze) on Twitter, take an educated
guess at his email address or look it up using [time-honored
methods](http://en.wikipedia.org/wiki/Whois). This article was published
on [kunststube.net](http://kunststube.net/). And no, there is no dirty
word in "Kunststube".

Enviado por jguzman el Lun, 07/28/2014 - 09:30



