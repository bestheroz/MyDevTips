# My Development Tips !!! #

## [Java] ##

### Integer == Integer ###

true case

	int int1 = 3;
	int int2 = 3;

	int1 == int2 // true

	Integer integer1 = 3;
	Integer integer2 = 3;

	integer1 == integer2 // true

Problem

	Integer integer1 = 300;
	Integer integer2 = 300;

	integer1 == integer2 // false

Tips

	Integer integer1 = 300;
	Integer integer2 = 300;

	integer1.intValue() == integer2.intValue() // true
	integer1.equals(integer2) // true, same as String

----------


### try { } finally { } ###

Unclean

	InputStream is = null;
	OutputStream is = null;

	try {
		is = new InputStream();
		os = new OutputStream();
		...

	} finally {
		try{
			if ( is != null) {
				is.close();
			}
		} catch ( Exception e ) {
			logger.warn( ExceptionUtils.getStackTrace(e) );
		}
		try{
			if ( is != null) {
				is.close();
			}
		} catch ( Exception e ) {
			logger.warn( ExceptionUtils.getStackTrace(e) );
		}
	}

Tips

	try (InputStream is = new InputStream(); OutputStream is = new OutputStream()) {
		...

	}

----------

### try { } catch(Exception e) { } ###

Problem

	try {
		throw ...;
	} catch (Exception e) {
		// All Exception
	}

Tips

	try {
		throw ...;
	} catch (Throwable e) {
		// All Throws
	}

----------

### org.apache.commons.lang3.StringUtils. Any ###

Unclean

	if ( StringUtils.equals(text1, "AAA") || StringUtils.equals(text1, "BBB") 
		&& !StringUtils.equals(text1, "CCC") && !StringUtils.equals(text1, "DDD") 
		&& !StringUtils.equals(text1, "EEE") ){ }

	if ( StringUtils.startsWith(text1, "AAA") || StringUtils.startsWith(text1, "BBB") 
		&& !StringUtils.endsWith(text1, "CCC") && !StringUtils.endsWith(text1, "DDD") 
		&& !StringUtils.endsWith(text1, "EEE") ){ }

Tips

	if ( StringUtils.equalsAny(text1, "AAA", "BBB") 
		&& !StringUtils.equalsAny(text1, "CCC", "DDD", "EEE") ){ }

	if ( StringUtils.startsWithAny(text1, "AAA", "BBB") 
		&& !StringUtils.endsWith(text1, "CCC", "DDD", "EEE") ){ }

----------

### org.apache.commons.lang3.StringUtils.abbreviate ###

Unclean

	if( StringUtils.length( text1 ) > 2000 ){
		StringUtils.substring(text1, 0 , 2000) + "...";
	}

Tips

	StringUtils.abbreviate(formattedMessage, 2000);

### org.apache.commons.lang3.StringUtils.substringBefore, substringAfter ###

Unclean

	String email = "bestheroz@sk.com";
	if( StringUtils.indexOf( email, "@" ) != -1 ){
		StringUtils.substring( email, 0, StringUtils.indexOf( email, "@" )) // get Id
	}
	if( StringUtils.indexOf( email, "@" ) != -1 ){
		StringUtils.substring( email, StringUtils.indexOf( email, "@" )) // get domain
	}

Tips

	String email = "bestheroz@sk.com";
	StringUtils.substringBefore(email, "@"); // get Id
	StringUtils.substringAfter(email, "@"); // get domain

----------

## [JS] ##

### String contains() using indexOf() ###

Unclean

	var someText = 'javascript rules';
	if (someText.indexOf('javascript') !== -1) {
	}
		
	// or
	if (someText.indexOf('javascript') >= 0) {
	}

Tips

	var someText = 'text';

	!!~someText.indexOf('tex'); // someText contains "tex" - true
	!~someText.indexOf('tex'); // someText NOT contains "tex" - false
	~someText.indexOf('asd'); // someText doesn't contain "asd" - false
	~someText.indexOf('ext'); // someText contains "ext" - true

> http://www.jstips.co/en/javascript/even-simpler-way-of-using-indexof-as-a-contains-clause/

----------

### Safe string concatenation ###

Problem

	var one = 1;
	var two = 2;
	var three = '3';
	
	var result = one + two + three; //"33" instead of "123"

Tips

	var one = 1;
	var two = 2;
	var three = '3';
	
	var result = ''.concat(one, two, three); //"123"

> http://www.jstips.co/en/javascript/safe-string-concatenation/

----------

### enable chaining of functions ###

Unclean

	var data1 = '2018-12-31 23:59:59'; // Goal: 20181231235959
	data1 = removeAll(data1, '-');
	data1 = replaceAll(data1, ':', '');
	data1 = removeAll(data1, '  ');
	console.info(data1); // 20181231235959

Or Unclean

	var data1 = removeAll(replaceAll(removeAll('2018-12-31 23:59:59', '-'), ':', ''), ' ');
	console.info(data1); // 20181231235959

Tips

	function Date1(data1) {
	  this.data1 = data1;
	
	  this.removeAll = function(src) {
	     return this.replaceAll(src, '');
	  };
	
	  this.replaceAll = function(src, desc) {
	    return this.data1.split(src).join(desc);
	  };
	}
	
	var data1 = new Date1('John');
	data1.removeAll('-').replaceAll(':', '').removeAll(' ');

> http://www.jstips.co/en/javascript/return-objects-to-enable-chaining-of-functions/

----------

### Two ways to empty an array ###

Problem

list = [] assigns a reference to a new array to a variable, while any other references are unaffected. which means that references to the contents of the previous array are still kept in memory, leading to memory leaks.

		var list = [1, 2, 3, 4];
		function empty() {
		    //empty your array
		    list = [];
		}
		empty();

Tips

list.length = 0 deletes everything in the array, which does hit other references.

		var list = [1, 2, 3, 4];
		function empty() {
		    //empty your array
		    list.length = 0;
		}
		empty();

Example

		var foo = [1,2,3];
		var bar = [1,2,3];
		var foo2 = foo;
		var bar2 = bar;
		foo = [];
		bar.length = 0;
		console.log(foo, bar, foo2, bar2);
		
		// [] [] [1, 2, 3] []

> http://www.jstips.co/en/javascript/two-ways-to-empty-an-array/

----------

### Converting to number fast way ###

Tips

	var one = '1';
	
	var numberOne = +one; // Number 1

	var negativeNumberOne = -one; // Number -1

> http://www.jstips.co/en/javascript/converting-to-number-fast-way/

----------

### set default values ###

Unclean

	function documentTitle(theTitle) {
		​if (!theTitle) {
	  		theTitle  = "Untitled Document";
	  	}

		if(condition){
		    dosomething();
		}
	}

Tips

	function documentTitle(theTitle)
	  	theTitle  = theTitle || "Untitled Document";

		condition && dosomething();
	}

	var a;

	console.log(a); //undefined
	
	a = a || 'default value';
	
	console.log(a); //default value
	
	a = a || 'new value';
	
	console.log(a); //default value

> http://javascriptissexy.com/12-simple-yet-powerful-javascript-tips/

> http://www.jstips.co/en/javascript/three-useful-hacks/

----------

### Converting truthy/falsy values to boolean ###

	!!"" // false
	!!0 // false
	!!null // false
	!!undefined // false
	!!NaN // false
	
	!!"hello" // true
	!!1 // true
	!!{} // true
	!![] // true

> http://www.jstips.co/en/javascript/converting-truthy-falsy-values-to-boolean/

----------

### Null, Undefined, Empty Checks ###

Unclean

	var variable2 = '';
	if (variable1 !== null || variable1 !== undefined || variable1 !== '') {
	     variable2 = variable1;
	}

Tips

	var variable2 = variable1  || '';

*P.S.: If variable1 is a number, then first check if it is 0.*

> http://www.jstips.co/en/javascript/assignment-shorthands/

----------


### Passing an empty parameter to a method ###

Problem

	method('parameter1', , 'parameter3');
	> Uncaught SyntaxError: Unexpected token ,

solution

	method('parameter1', null, 'parameter3') // or
	method('parameter1', undefined, 'parameter3');

Tips

	method(...['parameter1', , 'parameter3']); // works!

> http://www.jstips.co/en/javascript/3-array-hacks/

----------

### Why you should use Object.is() in equality comparison ###

Problem

	0 == ' ' //true
	null == undefined //true
	[1] == true //true

true case

	0 === ' ' //false
	null === undefined //false
	[1] === true //false
	NaN === NaN //false

Tips

	Object.is(0 , ' '); //false
	Object.is(null, undefined); //false
	Object.is([1], true); //false
	Object.is(NaN, NaN); //true

**IE doesn't support Object.is() Function.**

> http://www.jstips.co/en/javascript/why-you-should-use-Object.is()-in-equality-comparison/

----------

### startsWith, endsWith, contains Function in JS ###

Unclean

	var example1 = 'Hello. Hi. Bye.';
	if( example1.indexOf('Hell') === 0 ){ // startsWith

	}
	if( exmaple.indexOf('Bye.') === example1.length - 'Bye.'.length ){ // endsWith 
		
	}
	if( example.indexOf('Hi') !== -1 ){ // contains

	}

Tips

> Use lodash.js (~24kB)

	var example1 = 'Hello. Hi. Bye.';
	if( _.startsWith(example1, 'Hell') ){ // startsWith

	}
	if( _.endsWith(example1, 'Bye.') ){ // endsWith 
		
	}
	if( _.includes(example1, 'Hi') ){ // contains

	}

----------


### "(Double quote) or '(Single quote)

	$('#table1').html('');
	$('#table1').html("");

Little Problem

	<tr class='row'><td class='data'></td></tr>
	<input type='text' value=''>

	$("#table1").html("<tr class='row'><td class='data'></td></tr>");
	$("#table1").html("<input type='text' value=''>");

Tips

	<tr class="row"><td class="data"></td></tr>
	<input type="text" value="">

	$('#table1').html('<tr class="row"><td class="data"></td></tr>');
	$('#table1').html('<input type="text" value="">');

----------

### property, var, const, let ###

	propertyTest = "1";
	delete propertyTest; // return true;
	
	var varTest = "2"
	delete varTest; // return false;
	
	const constTest = "3";
	delete constTest; // return false;
	
	let letTest = "4";
	delete letTest; // return false;

Problem

	function fun1(){
		propertyTest = "1"; // Bad Object;
		var varTest = "2"
		const constTest = "3";
		let letTest = "4";
	}

Problem

	function fun1(){
		var returnCode = 0;

		if(true){
			var returnCode = 3;
			for(i=0;i<3;i++){
				returnCode++;
			}
		}

		return returnCode; // ?
	}

Tips
	
	function fun1(){
		let returnCode = 0;

		if(true){
			let returnCode = 3;
			for(i=0;i<3;i++){
				returnCode++;
			}
		}

		return returnCode; // ?
	}

> **const, let are supported since IE 11**

## [JSP] ##

### form reset ###

Problem

	<form id="form1">
		<input type="hidden" id="input1" />
		<input type="hidden" id="input2" />
		<input type="hidden" id="input3" />
	</form>

	<script>
		$('#form1')[0].reset(); // does not work
	</script>

Tips

	<form id="form1">
		<input type="text" id="input1" style="display: none;" />
		<input type="text" id="input2" style="display: none;" />
		<input type="text" id="input3" style="display: none;" />
	</form>

	<script>
		$('#form1')[0].reset(); // works!
	</script>

----------

### JSTL Fault ###

Problem

	<input text="text" value="${value1}">

Step 1. Problem
 
	String value1 = "Hello, My name is \"Kim\".";
	<input text="text" value="${value1}">
	<input text="text" value="Hello, My name is \"Kim\".">

Step 2. Little Problem

	// Tag " -> '
	String value1 = "Hello, My name is \"Kim\".";
	<input text='text' value='${value1}'>
	<input text='text' value='Hello, My name is \"Kim\".'>

> [JS] - "(Double quote) or '(Single quote) 항목 참조

Step 3. Problem

	// value " -> '
	String value1 = "Hello, My name is 'Kim'.";
	<input text='text' value='${value1}'>
	<input text='text' value='Hello, My name is 'Kim'.'>

Step 4. Problem

	String value1 = "Hello, Doctor.

					My name is 'Kim'.";
	<input text="text" value="${value1}">
	<input text="text" value="Hello, Doctor.

					My name is 'Kim'.">

Another Problem

	var var1 = '${value}';
	var var2 = ${board_no};

Another Problem

	Stop the all page's function when jstl has problem;

Tips

	// Do not use.

----------
