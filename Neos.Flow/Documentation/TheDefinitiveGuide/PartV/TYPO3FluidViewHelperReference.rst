.. _`TYPO3 Fluid ViewHelper Reference`:

TYPO3 Fluid ViewHelper Reference
================================

This reference was automatically generated from code on 2021-08-25


.. _`TYPO3 Fluid ViewHelper Reference: f:alias`:

f:alias
-------

Declares new variables which are aliases of other variables.
Takes a "map"-Parameter which is an associative array which defines the shorthand mapping.

The variables are only declared inside the <f:alias>...</f:alias>-tag. After the
closing tag, all declared variables are removed again.

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\AliasViewHelper




Arguments
*********

* ``map`` (array): Array that specifies which variables should be mapped to which alias




Examples
********

**Single alias**::

	<f:alias map="{x: 'foo'}">{x}</f:alias>


Expected result::

	foo


**Multiple mappings**::

	<f:alias map="{x: foo.bar.baz, y: foo.bar.baz.name}">
	  {x.name} or {y}
	</f:alias>


Expected result::

	[name] or [name]
	depending on {foo.bar.baz}




.. _`TYPO3 Fluid ViewHelper Reference: f:cache.disable`:

f:cache.disable
---------------

ViewHelper to disable template compiling

Inserting this ViewHelper at any point in the template,
including inside conditions which do not get rendered,
will forcibly disable the caching/compiling of the full
template file to a PHP class.

Use this if for whatever reason your platform is unable
to create or load PHP classes (for example on read-only
file systems or when using an incompatible default cache
backend).

Passes through anything you place inside the ViewHelper,
so can safely be used as container tag, as self-closing
or with inline syntax - all with the same result.

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\Cache\\DisableViewHelper





.. _`TYPO3 Fluid ViewHelper Reference: f:cache.static`:

f:cache.static
--------------

ViewHelper to force compiling to a static string

Used around chunks of template code where you want the
output of said template code to be compiled to a static
string (rather than a collection of compiled nodes, as
is the usual behavior).

The effect is that none of the child ViewHelpers or nodes
used inside this tag will be evaluated when rendering the
template once it is compiled. It will essentially replace
all logic inside the tag with a plain string output.

Works by turning the `compile` method into a method that
renders the child nodes and returns the resulting content
directly as a string variable.

You can use this with great effect to further optimise the
performance of your templates: in use cases where chunks of
template code depend on static variables (like thoese in
{settings} for example) and those variables never change,
and the template uses no other dynamic variables, forcing
the template to compile that chunk to a static string can
save a lot of operations when rendering the compiled template.

NB: NOT TO BE USED FOR CACHING ANYTHING OTHER THAN STRING-
COMPATIBLE OUTPUT!

USE WITH CARE! WILL PRESERVE EVERYTHING RENDERED, INCLUDING
POTENTIALLY SENSITIVE DATA CONTAINED IN OUTPUT!

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\Cache\\StaticViewHelper





.. _`TYPO3 Fluid ViewHelper Reference: f:cache.warmup`:

f:cache.warmup
--------------

ViewHelper to insert variables which only apply during
cache warmup and only apply if no other variables are
specified for the warmup process.

If a chunk of template code is impossible to compile
without additional variables, for example when rendering
sections or partials using dynamic names, you can use this
ViewHelper around that chunk and specify a set of variables
which will be assigned only while compiling the template
and only when this is done as part of cache warmup. The
template chunk can then be compiled using those default
variables.

Note: this does not imply that only those variable values
will be used by the compiled template. It only means that
DEFAULT values of vital variables will be present during
compiling.

If you find yourself completely unable to properly warm up
a specific template file even with use of this ViewHelper,
then you can consider using `f:cache.disable` to prevent
the template compiler from even attempting to compile it.

USE WITH CARE! SOME EDGE CASES OF FOR EXAMPLE VIEWHELPERS
WHICH REQUIRE SPECIAL VARIABLE TYPES MAY NOT BE SUPPORTED
HERE DUE TO THE RUDIMENTARY NATURE OF VARIABLES YOU DEFINE.

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\Cache\\WarmupViewHelper




Arguments
*********

* ``variables`` (array, *optional*): Array of variables to assign ONLY when compiling. See main class documentation.




.. _`TYPO3 Fluid ViewHelper Reference: f:case`:

f:case
------

Case view helper that is only usable within the SwitchViewHelper.

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\CaseViewHelper




Arguments
*********

* ``value`` (mixed): Value to match in this case




.. _`TYPO3 Fluid ViewHelper Reference: f:comment`:

f:comment
---------

This ViewHelper prevents rendering of any content inside the tag
Note: Contents of the comment will still be **parsed** thus throwing an
Exception if it contains syntax errors. You can put child nodes in
CDATA tags to avoid this.

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\CommentViewHelper





Examples
********

**Commenting out fluid code**::

	Before
	<f:comment>
	  This is completely hidden.
	  <f:debug>This does not get rendered</f:debug>
	</f:comment>
	After


Expected result::

	Before
	After


**Prevent parsing**::

	<f:comment><![CDATA[
	 <f:some.invalid.syntax />
	]]></f:comment>




.. _`TYPO3 Fluid ViewHelper Reference: f:count`:

f:count
-------

This ViewHelper counts elements of the specified array or countable object.

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\CountViewHelper




Arguments
*********

* ``subject`` (array, *optional*): Countable subject, array or \Countable




Examples
********

**Count array elements**::

	<f:count subject="{0:1, 1:2, 2:3, 3:4}" />


Expected result::

	4


**inline notation**::

	{objects -> f:count()}


Expected result::

	10 (depending on the number of items in {objects})




.. _`TYPO3 Fluid ViewHelper Reference: f:cycle`:

f:cycle
-------

This ViewHelper cycles through the specified values.
This can be often used to specify CSS classes for example.
**Note:** To achieve the "zebra class" effect in a loop you can also use the "iteration" argument of the **for** ViewHelper.

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\CycleViewHelper




Arguments
*********

* ``values`` (array, *optional*): The array or object implementing \ArrayAccess (for example \SplObjectStorage) to iterated over

* ``as`` (strong): The name of the iteration variable




Examples
********

**Simple**::

	<f:for each="{0:1, 1:2, 2:3, 3:4}" as="foo"><f:cycle values="{0: 'foo', 1: 'bar', 2: 'baz'}" as="cycle">{cycle}</f:cycle></f:for>


Expected result::

	foobarbazfoo


**Alternating CSS class**::

	<ul>
	  <f:for each="{0:1, 1:2, 2:3, 3:4}" as="foo">
	    <f:cycle values="{0: 'odd', 1: 'even'}" as="zebraClass">
	      <li class="{zebraClass}">{foo}</li>
	    </f:cycle>
	  </f:for>
	</ul>


Expected result::

	<ul>
	  <li class="odd">1</li>
	  <li class="even">2</li>
	  <li class="odd">3</li>
	  <li class="even">4</li>
	</ul>




.. _`TYPO3 Fluid ViewHelper Reference: f:debug`:

f:debug
-------

<code title="inline notation and custom title">
{object -> f:debug(title: 'Custom title')}
</code>
<output>
all properties of {object} nicely highlighted (with custom title)
</output>

<code title="only output the type">
{object -> f:debug(typeOnly: true)}
</code>
<output>
the type or class name of {object}
</output>

Note: This view helper is only meant to be used during development

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\DebugViewHelper




Arguments
*********

* ``typeOnly`` (boolean, *optional*): If TRUE, debugs only the type of variables

* ``levels`` (integer, *optional*): Levels to render when rendering nested objects/arrays

* ``html`` (boolean, *optional*): Render HTML. If FALSE, output is indented plaintext




Examples
********

**inline notation and custom title**::

	{object -> f:debug(title: 'Custom title')}


Expected result::

	all properties of {object} nicely highlighted (with custom title)


**only output the type**::

	{object -> f:debug(typeOnly: true)}


Expected result::

	the type or class name of {object}




.. _`TYPO3 Fluid ViewHelper Reference: f:defaultCase`:

f:defaultCase
-------------

A view helper which specifies the "default" case when used within the SwitchViewHelper.

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\DefaultCaseViewHelper





.. _`TYPO3 Fluid ViewHelper Reference: f:else`:

f:else
------

Else-Branch of a condition. Only has an effect inside of "If". See the If-ViewHelper for documentation.

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\ElseViewHelper




Arguments
*********

* ``if`` (boolean, *optional*): Condition expression conforming to Fluid boolean rules




Examples
********

**Output content if condition is not met**::

	<f:if condition="{someCondition}">
	  <f:else>
	    condition was not true
	  </f:else>
	</f:if>


Expected result::

	Everything inside the "else" tag is displayed if the condition evaluates to FALSE.
	Otherwise nothing is outputted in this example.




.. _`TYPO3 Fluid ViewHelper Reference: f:for`:

f:for
-----

Loop view helper which can be used to iterate over arrays.
Implements what a basic foreach()-PHP-method does.

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\ForViewHelper




Arguments
*********

* ``each`` (array): The array or \SplObjectStorage to iterated over

* ``as`` (string): The name of the iteration variable

* ``key`` (string, *optional*): Variable to assign array key to

* ``reverse`` (boolean, *optional*): If TRUE, iterates in reverse

* ``iteration`` (string, *optional*): The name of the variable to store iteration information (index, cycle, isFirst, isLast, isEven, isOdd)




Examples
********

**Simple Loop**::

	<f:for each="{0:1, 1:2, 2:3, 3:4}" as="foo">{foo}</f:for>


Expected result::

	1234


**Output array key**::

	<ul>
	  <f:for each="{fruit1: 'apple', fruit2: 'pear', fruit3: 'banana', fruit4: 'cherry'}" as="fruit" key="label">
	    <li>{label}: {fruit}</li>
	  </f:for>
	</ul>


Expected result::

	<ul>
	  <li>fruit1: apple</li>
	  <li>fruit2: pear</li>
	  <li>fruit3: banana</li>
	  <li>fruit4: cherry</li>
	</ul>


**Iteration information**::

	<ul>
	  <f:for each="{0:1, 1:2, 2:3, 3:4}" as="foo" iteration="fooIterator">
	    <li>Index: {fooIterator.index} Cycle: {fooIterator.cycle} Total: {fooIterator.total}{f:if(condition: fooIterator.isEven, then: ' Even')}{f:if(condition: fooIterator.isOdd, then: ' Odd')}{f:if(condition: fooIterator.isFirst, then: ' First')}{f:if(condition: fooIterator.isLast, then: ' Last')}</li>
	  </f:for>
	</ul>


Expected result::

	<ul>
	  <li>Index: 0 Cycle: 1 Total: 4 Odd First</li>
	  <li>Index: 1 Cycle: 2 Total: 4 Even</li>
	  <li>Index: 2 Cycle: 3 Total: 4 Odd</li>
	  <li>Index: 3 Cycle: 4 Total: 4 Even Last</li>
	</ul>




.. _`TYPO3 Fluid ViewHelper Reference: f:format.cdata`:

f:format.cdata
--------------

Outputs an argument/value without any escaping and wraps it with CDATA tags.

PAY SPECIAL ATTENTION TO SECURITY HERE (especially Cross Site Scripting),
as the output is NOT SANITIZED!

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\Format\\CdataViewHelper




Arguments
*********

* ``value`` (mixed, *optional*): The value to output




Examples
********

**Child nodes**::

	<f:format.cdata>{string}</f:format.cdata>


Expected result::

	<![CDATA[(Content of {string} without any conversion/escaping)]]>


**Value attribute**::

	<f:format.cdata value="{string}" />


Expected result::

	<![CDATA[(Content of {string} without any conversion/escaping)]]>


**Inline notation**::

	{string -> f:format.cdata()}


Expected result::

	<![CDATA[(Content of {string} without any conversion/escaping)]]>




.. _`TYPO3 Fluid ViewHelper Reference: f:format.htmlspecialchars`:

f:format.htmlspecialchars
-------------------------

Applies htmlspecialchars() escaping to a value

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\Format\\HtmlspecialcharsViewHelper




Arguments
*********

* ``value`` (string, *optional*): Value to format

* ``keepQuotes`` (boolean, *optional*): If TRUE quotes will not be replaced (ENT_NOQUOTES)

* ``encoding`` (string, *optional*): Encoding

* ``doubleEncode`` (boolean, *optional*): If FALSE html entities will not be encoded




.. _`TYPO3 Fluid ViewHelper Reference: f:format.printf`:

f:format.printf
---------------

A view helper for formatting values with printf. Either supply an array for
the arguments or a single value.
See http://www.php.net/manual/en/function.sprintf.php

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\Format\\PrintfViewHelper




Arguments
*********

* ``value`` (string, *optional*): String to format

* ``arguments`` (array, *optional*): The arguments for vsprintf




Examples
********

**Scientific notation**::

	<f:format.printf arguments="{number: 362525200}">%.3e</f:format.printf>


Expected result::

	3.625e+8


**Argument swapping**::

	<f:format.printf arguments="{0: 3, 1: 'Kasper'}">%2$s is great, TYPO%1$d too. Yes, TYPO%1$d is great and so is %2$s!</f:format.printf>


Expected result::

	Kasper is great, TYPO3 too. Yes, TYPO3 is great and so is Kasper!


**Single argument**::

	<f:format.printf arguments="{1: 'TYPO3'}">We love %s</f:format.printf>


Expected result::

	We love TYPO3


**Inline notation**::

	{someText -> f:format.printf(arguments: {1: 'TYPO3'})}


Expected result::

	We love TYPO3




.. _`TYPO3 Fluid ViewHelper Reference: f:format.raw`:

f:format.raw
------------

Outputs an argument/value without any escaping. Is normally used to output
an ObjectAccessor which should not be escaped, but output as-is.

PAY SPECIAL ATTENTION TO SECURITY HERE (especially Cross Site Scripting),
as the output is NOT SANITIZED!

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\Format\\RawViewHelper




Arguments
*********

* ``value`` (mixed, *optional*): The value to output




Examples
********

**Child nodes**::

	<f:format.raw>{string}</f:format.raw>


Expected result::

	(Content of {string} without any conversion/escaping)


**Value attribute**::

	<f:format.raw value="{string}" />


Expected result::

	(Content of {string} without any conversion/escaping)


**Inline notation**::

	{string -> f:format.raw()}


Expected result::

	(Content of {string} without any conversion/escaping)




.. _`TYPO3 Fluid ViewHelper Reference: f:groupedFor`:

f:groupedFor
------------

Grouped loop view helper.
Loops through the specified values.

The groupBy argument also supports property paths.

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\GroupedForViewHelper




Arguments
*********

* ``each`` (array): The array or \SplObjectStorage to iterated over

* ``as`` (string): The name of the iteration variable

* ``groupBy`` (string): Group by this property

* ``groupKey`` (string, *optional*): The name of the variable to store the current group




Examples
********

**Simple**::

	<f:groupedFor each="{0: {name: 'apple', color: 'green'}, 1: {name: 'cherry', color: 'red'}, 2: {name: 'banana', color: 'yellow'}, 3: {name: 'strawberry', color: 'red'}}" as="fruitsOfThisColor" groupBy="color">
	  <f:for each="{fruitsOfThisColor}" as="fruit">
	    {fruit.name}
	  </f:for>
	</f:groupedFor>


Expected result::

	apple cherry strawberry banana


**Two dimensional list**::

	<ul>
	  <f:groupedFor each="{0: {name: 'apple', color: 'green'}, 1: {name: 'cherry', color: 'red'}, 2: {name: 'banana', color: 'yellow'}, 3: {name: 'strawberry', color: 'red'}}" as="fruitsOfThisColor" groupBy="color" groupKey="color">
	    <li>
	      {color} fruits:
	      <ul>
	        <f:for each="{fruitsOfThisColor}" as="fruit" key="label">
	          <li>{label}: {fruit.name}</li>
	        </f:for>
	      </ul>
	    </li>
	  </f:groupedFor>
	</ul>


Expected result::

	<ul>
	  <li>green fruits
	    <ul>
	      <li>0: apple</li>
	    </ul>
	  </li>
	  <li>red fruits
	    <ul>
	      <li>1: cherry</li>
	    </ul>
	    <ul>
	      <li>3: strawberry</li>
	    </ul>
	  </li>
	  <li>yellow fruits
	    <ul>
	      <li>2: banana</li>
	    </ul>
	  </li>
	</ul>




.. _`TYPO3 Fluid ViewHelper Reference: f:if`:

f:if
----

This view helper implements an if/else condition.

**Conditions:**

As a condition is a boolean value, you can just use a boolean argument.
Alternatively, you can write a boolean expression there.
Boolean expressions have the following form:
XX Comparator YY
Comparator is one of: ==, !=, <, <=, >, >= and %
The % operator converts the result of the % operation to boolean.

XX and YY can be one of:
- number
- Object Accessor
- Array
- a ViewHelper
- string

::

  <f:if condition="{rank} > 100">
    Will be shown if rank is > 100
  </f:if>
  <f:if condition="{rank} % 2">
    Will be shown if rank % 2 != 0.
  </f:if>
  <f:if condition="{rank} == {k:bar()}">
    Checks if rank is equal to the result of the ViewHelper "k:bar"
  </f:if>
  <f:if condition="{foo.bar} == 'stringToCompare'">
    Will result in true if {foo.bar}'s represented value equals 'stringToCompare'.
  </f:if>

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\IfViewHelper




Arguments
*********

* ``then`` (mixed, *optional*): Value to be returned if the condition if met.

* ``else`` (mixed, *optional*): Value to be returned if the condition if not met.

* ``condition`` (boolean, *optional*): Condition expression conforming to Fluid boolean rules




Examples
********

**Basic usage**::

	<f:if condition="somecondition">
	  This is being shown in case the condition matches
	</f:if>


Expected result::

	Everything inside the <f:if> tag is being displayed if the condition evaluates to TRUE.


**If / then / else**::

	<f:if condition="somecondition">
	  <f:then>
	    This is being shown in case the condition matches.
	  </f:then>
	  <f:else>
	    This is being displayed in case the condition evaluates to FALSE.
	  </f:else>
	</f:if>


Expected result::

	Everything inside the "then" tag is displayed if the condition evaluates to TRUE.
	Otherwise, everything inside the "else"-tag is displayed.


**inline notation**::

	{f:if(condition: someCondition, then: 'condition is met', else: 'condition is not met')}


Expected result::

	The value of the "then" attribute is displayed if the condition evaluates to TRUE.
	Otherwise, everything the value of the "else"-attribute is displayed.




.. _`TYPO3 Fluid ViewHelper Reference: f:inline`:

f:inline
--------

Inline Fluid rendering ViewHelper

Renders Fluid code stored in a variable, which you normally would
have to render before assigning it to the view. Instead you can
do the following (note, extremely simplified use case):

     $view->assign('variable', 'value of my variable');
     $view->assign('code', 'My variable: {variable}');

And in the template:

     {code -> f:inline()}

Which outputs:

     My variable: value of my variable

You can use this to pass smaller and dynamic pieces of Fluid code
to templates, as an alternative to creating new partial templates.

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\InlineViewHelper




Arguments
*********

* ``code`` (string, *optional*): Fluid code to be rendered as if it were part of the template rendering it. Can be passed as inline argument or tag content




.. _`TYPO3 Fluid ViewHelper Reference: f:layout`:

f:layout
--------

With this tag, you can select a layout to be used for the current template.

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\LayoutViewHelper




Arguments
*********

* ``name`` (string, *optional*): Name of layout to use. If none given, "Default" is used.




.. _`TYPO3 Fluid ViewHelper Reference: f:or`:

f:or
----

If content is empty use alternative text

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\OrViewHelper




Arguments
*********

* ``content`` (mixed, *optional*): Content to check if empty

* ``alternative`` (mixed, *optional*): Alternative if content is empty

* ``arguments`` (array, *optional*): Arguments to be replaced in the resulting string, using sprintf




.. _`TYPO3 Fluid ViewHelper Reference: f:render`:

f:render
--------

A ViewHelper to render a section, a partial, a specified section in a partial
or a delegate ParsedTemplateInterface implementation.

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\RenderViewHelper




Arguments
*********

* ``section`` (string, *optional*): Section to render - combine with partial to render section in partial

* ``partial`` (string, *optional*): Partial to render, with or without section

* ``delegate`` (string, *optional*): Optional PHP class name of a permanent, included-in-app ParsedTemplateInterface implementation to override partial/section

* ``renderable`` (TYPO3Fluid\Fluid\Core\Rendering\RenderableInterface, *optional*): Instance of a RenderableInterface implementation to be rendered

* ``arguments`` (array, *optional*): Array of variables to be transferred. Use {_all} for all variables

* ``optional`` (boolean, *optional*): If TRUE, considers the *section* optional. Partial never is.

* ``default`` (mixed, *optional*): Value (usually string) to be displayed if the section or partial does not exist

* ``contentAs`` (string, *optional*): If used, renders the child content and adds it as a template variable with this name for use in the partial/section




Examples
********

**Rendering partials**::

	<f:render partial="SomePartial" arguments="{foo: someVariable}" />


Expected result::

	the content of the partial "SomePartial". The content of the variable {someVariable} will be available in the partial as {foo}


**Rendering sections**::

	<f:section name="someSection">This is a section. {foo}</f:section>
	<f:render section="someSection" arguments="{foo: someVariable}" />


Expected result::

	the content of the section "someSection". The content of the variable {someVariable} will be available in the partial as {foo}


**Rendering recursive sections**::

	<f:section name="mySection">
	 <ul>
	   <f:for each="{myMenu}" as="menuItem">
	     <li>
	       {menuItem.text}
	       <f:if condition="{menuItem.subItems}">
	         <f:render section="mySection" arguments="{myMenu: menuItem.subItems}" />
	       </f:if>
	     </li>
	   </f:for>
	 </ul>
	</f:section>
	<f:render section="mySection" arguments="{myMenu: menu}" />


Expected result::

	<ul>
	  <li>menu1
	    <ul>
	      <li>menu1a</li>
	      <li>menu1b</li>
	    </ul>
	  </li>
	[...]
	(depending on the value of {menu})


**Passing all variables to a partial**::

	<f:render partial="somePartial" arguments="{_all}" />


Expected result::

	the content of the partial "somePartial".
	Using the reserved keyword "_all", all available variables will be passed along to the partial


**Rendering via a delegate ParsedTemplateInterface implementation w/ custom arguments**::

	<f:render delegate="My\Special\ParsedTemplateImplementation" arguments="{_all}" />


Expected result::

	Whichever output was generated by calling My\Special\ParsedTemplateImplementation->render()
	with cloned RenderingContextInterface $renderingContext as only argument and content of arguments
	assigned in VariableProvider of cloned context. Supports all other input arguments including
	recursive rendering, contentAs argument, default value etc.
	Note that while ParsedTemplateInterface supports returning a Layout name, this Layout will not
	be respected when rendering using this method. Only the `render()` method will be called!




.. _`TYPO3 Fluid ViewHelper Reference: f:section`:

f:section
---------

A ViewHelper to declare sections in templates for later use with e.g. the RenderViewHelper.

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\SectionViewHelper




Arguments
*********

* ``name`` (string): Name of the section




Examples
********

**Rendering sections**::

	<f:section name="someSection">This is a section. {foo}</f:section>
	<f:render section="someSection" arguments="{foo: someVariable}" />


Expected result::

	the content of the section "someSection". The content of the variable {someVariable} will be available in the partial as {foo}


**Rendering recursive sections**::

	<f:section name="mySection">
	 <ul>
	   <f:for each="{myMenu}" as="menuItem">
	     <li>
	       {menuItem.text}
	       <f:if condition="{menuItem.subItems}">
	         <f:render section="mySection" arguments="{myMenu: menuItem.subItems}" />
	       </f:if>
	     </li>
	   </f:for>
	 </ul>
	</f:section>
	<f:render section="mySection" arguments="{myMenu: menu}" />


Expected result::

	<ul>
	  <li>menu1
	    <ul>
	      <li>menu1a</li>
	      <li>menu1b</li>
	    </ul>
	  </li>
	[...]
	(depending on the value of {menu})




.. _`TYPO3 Fluid ViewHelper Reference: f:spaceless`:

f:spaceless
-----------

Space Removal ViewHelper

Removes redundant spaces between HTML tags while
preserving the whitespace that may be inside HTML
tags. Trims the final result before output.

Heavily inspired by Twig's corresponding node type.

<code title="Usage of f:spaceless">
<f:spaceless>
<div>
    <div>
        <div>text

text</div>
    </div>
</div>
</code>
<output>
<div><div><div>text

text</div></div></div>
</output>

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\SpacelessViewHelper





Examples
********

**Usage of f:spaceless**::

	<f:spaceless>
	<div>
	    <div>
	        <div>text
	
	text</div>
	    </div>
	</div>


Expected result::

	<div><div><div>text
	
	text</div></div></div>




.. _`TYPO3 Fluid ViewHelper Reference: f:switch`:

f:switch
--------

Switch view helper which can be used to render content depending on a value or expression.
Implements what a basic switch()-PHP-method does.

An optional default case can be specified which is rendered if none of the "f:case" conditions matches.

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\SwitchViewHelper




Arguments
*********

* ``expression`` (mixed): Expression to switch




Examples
********

**Simple Switch statement**::

	<f:switch expression="{person.gender}">
	  <f:case value="male">Mr.</f:case>
	  <f:case value="female">Mrs.</f:case>
	  <f:defaultCase>Mr. / Mrs.</f:defaultCase>
	</f:switch>


Expected result::

	"Mr.", "Mrs." or "Mr. / Mrs." (depending on the value of {person.gender})




.. _`TYPO3 Fluid ViewHelper Reference: f:then`:

f:then
------

"THEN" -> only has an effect inside of "IF". See If-ViewHelper for documentation.

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\ThenViewHelper





.. _`TYPO3 Fluid ViewHelper Reference: f:variable`:

f:variable
----------

Variable assigning ViewHelper

Assigns one template variable which will exist also
after the ViewHelper is done rendering, i.e. adds
template variables.

If you require a variable assignment which does not
exist in the template after a piece of Fluid code
is rendered, consider using `f:alias` instead.

Usages:

    {f:variable(name: 'myvariable', value: 'some value')}
    <f:variable name="myvariable">some value</f:variable>
    {oldvariable -> f:format.htmlspecialchars() -> f:variable(name: 'newvariable')}
    <f:variable name="myvariable"><f:format.htmlspecialchars>{oldvariable}</f:format.htmlspecialchars></f:variable>

:Implementation: TYPO3Fluid\\Fluid\\ViewHelpers\\VariableViewHelper




Arguments
*********

* ``value`` (mixed, *optional*): Value to assign. If not in arguments then taken from tag content

* ``name`` (string): Name of variable to create



