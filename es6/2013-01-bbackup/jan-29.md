# January 29 2013 Meeting Notes

John Neumann (JN), Norbert Lindenberg (NL), Allen Wirfs-Brock (AWB), Rick Waldron (RW), Waldemar Horwat (WH), Tim Disney (TD), Eric Ferraiuolo (EF), Sam Tobin-Hochstadt (STH), Erik Arvidsson (EA), Brian Terlson (BT), Luke Hoban (LH), Matt Sweeney (MS), Doug Crockford (DC), Yehuda Katz (YK), Nebojša Ćirić (NC), Brendan Eich (BE)


JN: …Welcome and introduction, agenda. Discussion of TG/TC procedural changes. Please review and feedback.

…Adoption of the Agenda. 

AWB: Are there items that need to be discussed that aren't yet on the agenda?

STH: Will provide an update on Modules

## Added: 4.14 Modules Update

AWB: Discussion about Proxy? 

STH: A lot of discussion about the interaction of Proxy and Private Names

…Should we wait for Mark Miller?

AWB: Nowhere near consensus on any point of discussion from the mailing list.

## Added: 4.15 Proxy Issues

LH: (to AWB) did you want to give an update on the spec?

## Added: 4.16 Spec Status Update 

JN: We should have a discussion regarding the list of items that will actually end up in ES6 and determine exact ES6 additions.

RW: Let's set this as an agenda item for the next meeting and try to reduce the number of submitted agenda items. This allows everyone the opportunity to prepare for the large scale discussion.

JN: Will add an agenda item for March.

…Brief discussion about features that may or may not be ready; STH identifies: event loop (dependency of modules, Object.observe)

AWB: Two issues: Semantic completeness of the designs and what does it take to implement these into the language. 

Mixed discussion.

JN summarizes as part of 4.16 discussion

## Agenda Approved

## November 2012 Minutes Approved 

## 4.16 Spec Status Update
(Presented by Allen Wirfs-Brock)

AWB: Revisits Nov 22, http://wiki.ecmascript.org/doku.php?id=harmony:specification_drafts

Discussion re: instanceof. 

EA: Checks and changes to implementations that use instanceof

STH: Expect slow down from use of instanceof?

AWB/EA: yes.

WH: Concerns, how do you tell if a regex is a regex

YH: Not with instanceof, use Object.prototype.toString

AWB: Shouldn't use instanceof

EA: This hook makes instanceof behave they way you expect it. 

AWB: Revisits Dec 21, 2012 Draft changes

Mixed discussion re: keeping up to date with the spec drafts, request for more technical-focused reviews.

## __proto__.

LH: Begins with raised points about __proto__)…

Mixed discussion about __proto__ semantics. 

AWB: There is an underlying feeling that we don't like this and we need to get over that.

LH/YK: Agree.

DC: Disagree.

YK, DC: Discussion about __proto__ reality. 

YK: We need consistency.

DC: We should not standardize and leave as is.

Impasse.

LH: Similar issues with block scoped function declaration incompatibilities.

WH: We should give up on __proto__ in the same way that we gave up on "with"

EA: IE is implementing for web compatibility

AWB: The defacto standard for mobile web uses __proto__

EA: IF we could go back, we could do it differently

YK: We just need compatibility

LH: We need to just suck it up and standardize

Mixed discussion re: user code and __proto__

AWB: (clarifies) Luke is requesting that we pin down the __proto__ details as soon as possible.

WH/DC: (have concerns, against standardizing __proto__)

YK/BE: Discussion re: interop with current implementations.

BE: (Review of latest changes to __proto__ in Firefox)

EA: Matches Safari

BE: __proto__ is configurable (can be deleted), accessor (getter and setter throw), reflection does not leak.

AWB: Involves magic

BE: Yes, but minimal. (Confirms that latest __proto__ is out in wild, Firefox)

WH: Clarify "poisoning"?

BE: When you call it, it throws.

…Discussion re: MOP semantics with __proto__

BE: Proxy has to stratify the MOP.

AWB: Another issue… Objects that are non-extensible, can you change __proto__? Specifically, now that we're talking about being able to change __proto__, what type of objects can be changed?

BE: Wait for Mark? 

AWB: Agree.

Hold until Mark Miller is present. 

(Further discussion)

BE: Let's get back to this with Mark present and come to a conclusion.


## 4.1 is/isnt operators
(Presented by Allen Wirfs-Brock)
http://wiki.ecmascript.org/doku.php?id=harmony:egal

AWB: The plan of record on the wiki is that there is an "is" and "isnt" operator. There are various discussions on the mailing list that refer to this, but also to Object.is and Object.isnt. We need a _final_ decision on the operator form.

YK: The discussion was to move the world from ==?

WH: No, cannot move from == to [egals]

YK: The differences between === and is are significantly small. 

BE: On behalf of Dave Herman and Jason Orendorff… Syntax needs to be worth the expense and there options.

General agreement.

WH: (question about new type NaNs)

AWB: No spec provisions, currently… 

…Hypothetical discussion about new "decimal" type with a "decimal  NaN" and the implications of NaN equality.


**Conclusion/Resolution**
Consensus on **No operator**

Do we want API? **Yes.**

What is the name? **Object.is(x, y)**
(NOT Object.isnt, ! is sufficient for negation)

If it's "all discriminating", should it discriminate +0/-0? **Yes**

NaN is _not_ different from NaN (all NaNs are equated).

Every observable value "is" itself and nothing else.

## 4.4 Is there are need for Number.isNaN if we have Object.is available?

BE: Keep the concerns separate. 

AWB: If we have Object.is, we don't actually need Number.isNaN

DC: Object.is is sufficient.

BE: (Whiteboard example)

RW: Punt on Number.isNaN for ES6, defer to ES7 hinging on need based on potential emergent library code.

BE: (Notes that isNaN, isFinite, isInteger)

(toInteger was mistakenly changed to toInt, discussion leads to revisiting March 29th 2012 notes where no rationale was recorded. https://mail.mozilla.org/pipermail/es-discuss/2012-March/021919.html) 



**Conclusion/Resolution**
Overturn the Number.toInteger=>toInt change. Restore as toInteger. (Brendan, can you fill in the rationale for this?)

Number.isNaN remains in ES6


## 4.2 SameValue for Map/Set equivalence?
## 4.3 Parameterize the equivalence operator for Map/Set?

AWB: For ES6, only specify a limited set of comparison operators?

Discussion re: arbitrary comparisons?

Hash Codes?

YK: If the goal is to avoid whole new design issues, stick to strict equality.

BE: Reminder that adding hash codes now is too late in the game.

YK/AWB: Concerns that maps with strict equality will trip people  up.

YK: Typically, Maps have custom equality capabilities.

AWB: (Reminder of hash code pitfalls) But _only_ strict equality will be a disaster, but can't introduce hash codes at this stage. 

AWB: Suggest the default: +0/-0 are the same; NaN is a value, that is equal to itself for Map/Set equivalence.

Assuming we have a mechanism to override…

Ideally, we'd want to set this in the constructor, but there is already a single optional initializer argument.

BE: Let's find a champion to work this out over the next two days.

AWB: I can do this.


**Conclusion/Resolution**
To revisit, pending proposal from champion. (AWB)

Must address the minimal requirements on the wiki



## 6 Second Edition of ECMA-402 (Internationalization)
Last Update: https://github.com/rwldrn/tc39-notes/blob/master/es6/2012-11/nov-27.md#internationalization-update

NC: Approved to continue work. APIs covered, people assigned to write strawman. Have API changes to be written into the spec, based on use cases that have emerged.

NL: Normalization… talked about 2 additional normalization forms. 
1. CF, Case folding 
2. NFKC, Normalization form, compatibility pre-composed case folding (Norbert, can you confirm this is correct?)

Norbert, I had a hard time hearing you at the end of the table. I apologize for talking bad notes.

NL: There is an implementation of Edition 1, not prefixed on v8

NC: Overrode …?

BE: Good so far?

NC: Have some bugs with performance, but can address.

EA: No issues with actual semantics? 

NC: No complaints.

JN: Schedule? On track?

NC/NL: So far, but rely on ES6.

Discussion about overlapping schedule. 

**Conclusion/Resolution**
Please provide a written summary for the meeting notes.

Notes from NL/NC:

December 12: Ecma GA approves ECMA-402
http://ecma-international.org/publications/standards/Ecma-402.htm


January 10: Google shipped first implementation without prefix in Chrome 24


December 14: Internationalization ad-hoc met

- discussed scope in more detail, assigned strawman writing

Since then: Strawmen written, but not reviewed yet:

- number parsing
 http://wiki.ecmascript.org/doku.php?id=globalization:numberparsing

- text segmentation
 http://wiki.ecmascript.org/doku.php?id=globalization:text_segmentation

- message formatting
 http://wiki.ecmascript.org/doku.php?id=globalization:messageformatting

Strawmen assigned, but not written yet:

- character properties (regex or API?)
- display names
- time duration formatting
- alphabetic index

API changes to be written up:

- normalization (language spec - add CF and NFKC_CF)
- case conversion
- time zone support
- script reordering in Collator
- pseudo-numbering systems in NumberFormat


## 4.5 Why standardizing on __proto__ and not __define(G|S)etter__, __lookup(G|S)etter__?


Rationale for not adding Object.setPrototypeOf


**Conclusion/Resolution**
Consensus that __define(G|S)etter__, __lookup(G|S)etter__ will not be standardized, nor added to an Appendix.



## 4.8 Refactored new operator and the @@create method
(Presented by Allen Wirfs-Brock)

**REQUEST SLIDES**

The Basic Issue:
* Object Allocation and object initialization are separable issues.
* Subclassable abstractions require program level control…


Why Doesn't this work?

```js
class Vector extends Array {
	constructor(…args) {
		super(…args);
	}
}

let a = new Array();
let v = new Vector();

a[5] = v[5] = 5;

a.length; // 6
v.length; // 0
```

**Because…**

* Array uses a special exotic object representation that changes the semancs of [[DefineOwnObject]].

* The object that new Vector creates and passes to the Vector constructor is an ordinary object, not an exotic Array object.

* Even with the super call, the Array constructor doesn’t transform its this object into an exotic Array


**First Solution**

* Use normal [[Construct]] for built-­‐ins. 
	- Move magic ini1aliza1on (internal data proper1es  and internal methods) into the constructor func1on,  post object alloca1on. 
* Internal data proper1es need to be expandos  (probably based upon private symbols) 
* Built-­‐in methods use internal data property sniffing  instead of [[Class]] brand check.

**Objections**

Ollie's Objections:
* Doesn't want internal data properties to be elands
* Implementors want to allocate different machine/C level data structures for different kinds of built-ins.

Allen's Objections:
* What about internal method conflicts? 

Jason's Objections: 
* More than one magic constructor can be applied to an object

```js
let d = new Date(); 
Map.call(date); 
console.log(d.getYear()); // 2013 
Map.prototype.set.call(d,”month”, “January”); 
console.log(Map.prototype.get.call(d,”Month”)) // January
```

**What do other dynamic OO languages do?**

* Separate object alloca1on and ini1aliza1on into  separate phases.
* The "shape" and special characteris1cs of an  object are fixed during the object alloca1on  phase.
  –  Kind of like what [[Construct]] does, but... 
* The alloca1on phase is defined as a separate  class method 
  –  Can be inherited, or over-ridden, or super-invoked by  subclasses.  

**Sounds Good,  Let’s see if it works for JavaScript**

* @@create is a well known symbol that when used as a property of  a constructor provides the alloca1on method for that constructor. 
* New definition of the ordinary [[Construct]] : 
			1. Let creator be [[Get]] of this constructors @@create property 
			2.  If creator is not undefined, then  
			a. Let obj be the result of calling creator with this constructor as its this value. 
			3.  Else, fallback implementation  
			a. Let obj be a new ordinary object with its [[Prototype]] initialized from this constructor's “prototype” property. 
			4. Call this constructor with obj as the this value and the original argument list.
			5.  Return obj. 

* Most constructors just inherit Function.prototype[@@create]  which allocates a new ordinary object with its [[Prototype]]  initialized from this.prototype (the constructor’s “prototype”  property). 
* `new Foo() <=> Foo.call(Foo[@@create]())`


TODO: Copy slide text to notes?


Discussion re: `class Foo extends ??` semantics discussed in July, https://github.com/rwldrn/tc39-notes/blob/master/es6/2012-07/july-26.md#maxmin-class-semantics



WH: What happens when you do Number[@@create]? 

AWB/EA: Creates a new wrapped Number object.

BE: new Number() <=> Number.call(Number[@@create]())

AWB: Built-in @@create methods are non-writable, non-configurable. Just like built-in "prototype" properties (From slides).

… Back to slides.

**@@create Also Useful for Application Classes**

* DIY Branding
```js
import $$create ...;
const $fooBrand = Symbol(true); // or use a WeakMap
export class Foo {
  isFoo() {
    return !!this[$fooBrand];
  }
}

Object.mixin( Foo, {
  [$$create]() {
    let obj = super();
    Object.defineProperty( obj, $fooBrand, {
      value: true
    });
    return obj;
  }
});
```

```js



```

WK: The @@create could be hijacked and used to brand some other object with my object's brand.

AWB: No different from hijacking from an Array or Date… 

WK: Good for branding case, not for security

AWB: (Gives example of how you might make it work). Explanation of creating Proxy based instances with @@create (see slides)


**ES5 Built-In Branding**

Consider ES5+ Reality:

```js
function T() {
	var obj = [];
	obj.__proto__ = T.prototype;
	return obj;
};

var t = new T();
t[5] = 5;

t.length; // 6
Array.isArray(t); // true
t.toString(); // [object Array]
```

Built-in branding is based on the shape and capabilities of the actual instance object.


**ES6: @@create Determines Branding**

* Array.isArray will report true for subclass instances that are built-in exotic Array objects. These are allocated using Array[@@create];

```js
let v = new class extends Array {};
Array.isArray(v); // true
```

* Unless over-ridden using the `@@toStringTag`, `{}.toString` will report the legacy [[Class]] for built-in subclass instances if they are allocated using a built-in @@create method.


WH: If you're call isArray and it returns true, you'd expect to call Array.prototype methods. 

AWB: No guarantees about the prototype chain 

MM: Even if Array is frozen, the Array.prototype is mutable and I can add my own malicious push, pop, join.

MM, AWB, WH: (Discussion of security concerns)


Agreement that none of this weakens any existing invariants.


**No Need for [[Class]] or [[BuiltinBrand]]**

* These are really just specification devices for talking about specific forms of objects.
* Spec has always also used language like "an Array object" or "a RegExp instance"
* In ES6 spec. all [[Class]] uses uses can replaced with language like:
	- "is an exotic array object" <=> [[Class]]=="Array"
	- "has a [[Match]] internal data property" <=> [[Class]]=="RegExp"

File:
[[Match]] => [[RegExpMatch]] 


**Testing the |this| value almost works**

function Foo() {
	…
}

**Conclusion/Resolution**
Consensus on @@create, allocation and initialization decoupling.



**Constructors need to be able to recognize initialized instances**

* Built-ins can do this via existing internal properties
* User code could roll its own 

**Probably better to formalize initialized state as part of ES object model**

* Add one bit of state to every object: initialized/unitialized.
* Built-in @@create methods set new object state to uninitialized.
* `Object.call(uninitObj)` and other built-in constructors set initialized this objects to initialized state.
* `Object.isInitialized(obj)` is a new method that only returns false if obj is an object that is in the initialized state.
* Object.create(), {}, [] and various built-in functions create objects that are already initialized (backwards compat)

**Semantics…**

```js
// Decoupling allocation and initialization:
// Calling new X()…
class X { 				// 1. X[@@create](), allocation
	constructor() { // 2. Initialization
		super();
		this.state = "ready";
	}
}
```


EA: Worth the additional bit? Is the need common enough to address with new language API surface?

What about @call?

MM: @construct? If one is specified, otherwise … super call vs. super construct (Mark—Sorry, I had trouble following this—can you fill this in? Thanks!)

MM: Are there any objections to the parallel @construct entry point.

YK: It makes less sense in the ES6 context, since classes are focused around defining a constructor…

MM: I see, then call is the odd-ball. Agree with the objection.

YK: @call makes sense in the class case, @construct makes sense in the function case.

AWB/YK: Need statics to make the @call 

Mixed discussion regarding "static"

Yehuda will draft a static strawman for tomorrow.

(Break)

YK: All functions have a call and construct property, I propose to expose them

AWB: All functions are constructible, when a function is "newer", something happens before the function is invoked. 

AWB/YK: (Discussion re: existing semantics of function call and construct)

BE: The idea of having state in objects for initialization… 

Tabled.



**Various Oddities/Backward Compat Issues, 1**

…Missed first slide…

**Various Oddities/Backward Compat Issues, 2**

* Array.prototype.concat
	- Currently always creates Array instance, for subclasses usually want subclass instance
		- Change to use subclass constructor to create subclass instances but only when this object is tagged as array subclass
	- Currently auto-spreads Array instance arguments
		- Similar to above, auto-spread tagged array subclass args.
* Must compatibly support this idiom: 
	- `[].concat.apply(Array.prototype, arguments);`
* Precomputing result length will support use with TypedArrays
* Similar result object handling for slice, splice, map, filter(?)

(Re: Bullet 1)
WH: Concerned about the proliferation of "types of an object"… class, prototype, type… species?

(Re: Bullet 2)
Discussion focuses around built-ins whose prototype property are first-born instances of that object, eg. `Array.prototype` is an `Array`. The desire was to remove this from ES6, however there a slew of issues that are not specifically edge cases rise to the surface.

**Conclusion/Resolution, Bullet 2**
Brendan to experiment with changing the prototype properties of Boolean, Number and String away from first born instances into ordinary object ([object Object]).

(Re: Bullet 4)
Discussion focuses on what type ("species") of object should be created by these methods when subclassing.

AWB: All of them, "species" is what you want, except in the case of map. For slice, splice and filter, you always want to operate on and return the "species" calling.

WH: (Doesn't want another "type" thing)

YK: (Doesn't think it's a problem)

AWB: You can always cast your object back into the type of your class with `this.constructor`.


Illustrating Bullet 4:
```js
class V extends Array {
  constructor(...args) {
    super(...args);
  }
}

var v, m;
v = new V(1,2,3, );
m = v.map(val => val * 2);
console.log( m instanceof V ); // false :(
```

**Various Oddities/Backward Compat Issues, 3**

* String.prototype.match, replace, search, split.
	- Currently spec'ed to directly use RegExp internal APIs which limit the ability to use them with RegExp subclasses that use alternative engines that don't expose those APIs
	- Refactor into public operations upon RegExp subclass
	- String methods delegate to RegExp methods


MM: Confirm that any symbols defined only need to be unique, not private?

AWB: Yes.



## 4.10 Data collection/analysis: Function-in-block, duplicate parameter names, const, strict mode, var let/let[x]=1, and maybe more
(Presented by Brian Terlson)

Brian, please submit slides for inclusion in notes.

Mixed discussion regarding the actual expectation of these cases.

Can the tests be expanded beyond the top 10k?

BE: w/r to FIB, should we go with strict mode standardization?

YK: Does it always leak the identifier?

MM: In different ways.

Discussion regarding language forking. 

LH: (Identifies a dark future that hinges on "use strict")

MM: That should be the future.

… 
YK: Are there intersections of existing implementations that can be spec'ed?

Mixed discussion.

Table discussion until tomorrow.













http://oksoclap.com/VsCQU4MN68