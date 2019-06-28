# Software principles

* Users → authors (of code) → implementors (of low-level system for code) → specifiers (of standards) → theoretical purity (absolute knowledge).
	* In that way - 'size' instead of 'fftSize' or 'bufferSize'. Nobody wants to know the details, no one remembers how something works.
	* `smoothing` instead of `smoothingTimeConstant`, if there are no smoothingTimeVariable and smoothingSpaceConstant.
	* handle(what, how) pattern of calling functions.
	* memorizable case first (users), elegant API second (authors), code cleanness third (implementors, frameworks), framework compatibility fourth (specifiers), separation of concerns and correctness last (purity).

* What gift to God the project should be?

* Moral principle: "What if everybody would do that?"

* Make it work, make it right, make it fast.

* Wrappers:
  * string
	* function
	* class
	* stream
	* CLI
	* web-component
	* jquery plugin
	* standalone package
	* browserify transform
	* other plugin. Implement base, then wrappers.

* Do not implement patterns before code. Any pattern applied before working code is complication and lowers the chance of success. Example - do not force buffers extend audio format, if no at least 3 use-cases.
	* Do not use libraries if you don’t need to. Plain JS is better.
	* Make things in shortest known way. Don’t use jquery selectors or css, if you can easily use DOM-ones. Don’t use classes, if `id` fits better. Don’t make excessive class names (BEM-like) to (allegedly) enhance performance.
	* Do not optimize beforehead: preoptimized code hides further possibilities of optimization. Better write code, viable to be optimized in a lot of different ways, to choose the best ones.
	* Do not create function if code is not reused at least 2 times. Scaffolding is indent’s task, not function.
	* Do not create method if it has no instance meaning or not to be used outside. That clutters API with no reason.

* Stick to conventions (make it right)

* ~~`lib` folder or table of contents in readme - something you do wrong with your package.

* Write self-documented verbose obvious code. Not short symbols, but easy-to-remember or guess phrases. font-size is better than .fs, .box-shadow is better than .bshad. JS is better than LiveScript in that.
	* Arrow functions, unfortunately, do not follow this :(

* Do not use static constructor methods, like `Class.create()`. That out of convention method creates confusion.

* Do not implement lots of interfaces - provide minimal dependless implementation. (YAGNI partly). Do not do stream, emitter, observable etc. These interfaces can be covered by separate components. Anyways it is not a task of a module. Implementing bunch of extraneous stuff forces consumers (users) include tons of redundant dependencies. Also it is harder to support later. Better split things apart, like audio-formant-stream.

* Make transparent API, read provide clear method of work with underneath API. Do not do magic or hide things. For example, gl-shader hides the way bind() works or how do we pass textures to it. That makes things obscure so that it is safer to use low-level API manually.

* Optimization rules
	* Best calc is absence of calc (precalc things, put waveforms to memory)
	* In repeating structures remove everything irrelated to straight operation (glsl emulator - any extra-thing inside a cycle, like getter/setter is doomed)
	* Do not calc things which are already calced (cooley tokey method of merging calculated frequency values)

* Understanding rules
	* Ask a question
	* Do debates - reasoning
	* Reproduce algorithm step by step
	* Understand formula, not just see magic in that

* Write code in highest possible quality, as if you write it for ages. As if it is polyfill, native API. No one need bad code, it’s untrustworthy.

* Keep it simple and stupid. Difficult to understand things are diffucult to use. Separation of Concerns. Thing should be easy to guess/understand. Get close to user’s guesstures/mental model.

* Do not overdo in code. E. g. buffer padding after any operation is bad, if it is used only in single branch beforehead. Implement only known ways, do not create [городить] structures in hope that they will polyfill and normalize magically any input case. That hits performance and unreliable.

* Use particles explanation for math. It helps understanding nature of sin/cos, of currents, of FT, of many other algorithms.

* git commit culture. One change - one commit. Directive messages. Clean code development is value.

* Code is like a grammar: no one likes mess and mistaken code. Write clean, beautiful, neat code.

* If you do questionable statement in code, comment the reason - who do need this, why this should be here.

* Name things by it’s sense. Do not make tricks, scalings and other fun stuff. Call them exactly as they are. Not search-results, but search-suggestions. Do not make generalizations in outputting information. Make concise, clear and precise names.

* Jquery: use details object when you need to pass some complex data structure. This case is both applicable in workers, jquery hooks/events etc.

* Do not make controllers creating element: you can't then bind another controllers to that element, you cannot even call the same controller second time.
	* Maybe it’s reasonable to use Backbone approach: create element, if it’s not passed.
	* Look at DOM as on land, and on controllers as on landlords.

* Notes and critical discussion in project is evolution. It’d be nice to have a lot of opponents though.
	* Q/A in research.js is super-efficient tool for perf/experiments/tests.

* If you’ve left project once, when you get back later you understand that now there’s nothing you understand. This happened with photoshopr, typer and others. Most certainly you’d like to rewrite it. Don’t leave project not have finished it.
	* Questionable. Sometimes you just need to leave stuff which isn’t growing.

* Dynamic rendering along with static one raises trouble of making ajax dynamic rerendering work.
	* Ractive tpl’s, as well as web-components ones requires to be rendered afterwards, when js being inited.
		* In this case we have to whether to have two versions of template or ignore one of versions.
		* Besides, graceful degradation (progressive enhancement) isn’t possible with that approach.
	* There’s a possible approach of generating proper ajax-glueing JS server-side, that way we can avoid duplicationg templates,
	initing JS right on the client not breaking JS.

* ✘ Use modernizr instead of own features detecting. It’s generic approach.
	* ✔ No - it’s too heavyweight. Use it’s snippets.
	* Or better use polyfills and don’t care about it.

* Always *keep any project*, any work *in working state*. Any commit should grant that project will start and do something. Do not start global refactoring. Go by way of minimal-possible working states. Make before simplier than difficult. It’s more efficacious.

* There’s a performance price to any code, especially if resulting size matters. Do not write sinonymical or utilitary code, recognizing input well. Do not sugar.
	* Value of your code is evaluated by application. If that API’s used at least more than once - that code is significant. Otherwise - throw it out, it is just a complexion and unnecessary maintenance.

* Avoid small and unclear comments.
	* Bad: Just a stub for technical needs.
	* Good: A stub for noname instances.

* Mind that closure compiler optimizes logical nodes, or tokens, not the length of your variables. It is sensless to avoid double function call if you have to form return object instead.

* Do git tags for passing tests stable versions. Mark them as releases in github.

* Make guided errors, do not break silently your components.
	* It’s bad practice, according to the web-components best practices. Keep to graceful degradation principle.

* Write code as a story
	* Begin with constructor, init, create
	* Place utils to the end
	* Group similar blocks together
	* Explain reasons to your code, not just place it silently

* OBSOLETE: Place designs to separate branch: not everyone needs your code with design and vice versa.
	* use .npmignore instead, switching branches is worrisome

* Make wrappers for lists, not the items: items can be interdependent so that you have to deal with one items before others, and you cannot unambiguously define the order.

* A trouble should get fast & minimal solution (test coverage), and only then - refactoring, to generalize tests (you’ll see picture more fully).

* Don’t use (raw) external modules in development. As far you can’t affect them, you’ll just waste time in case something goes wrong. Replace things only in need. Recall emmitt, merge, color.

* Start refactoring with difficult things, then generalize and cover simple ones.
	* Start sketching with easy things, then start refactoring.

* Ideal project name - 4 chars (tab size). Chainable things look wonderful.

* !? Do method names length proportional to it’s complexity.

* Function name must be a verb. Object must be a noun.

* Constant-like properties must be out, like static members; arguments must be dynamic.
	* `setFillColor` in canvas changes context’s global variable, it is not the fillRect's argument.

* Use present time for event names - if callback is called before event actually takes place. Use that predominantly. Use past time (contentLoaded, changed) - if event actually happened. set === change, but changed !== change.

* Don’t license code. Don’t spread the cancer. Your author right is unwithdrawable. Even MIT license is a redundant thing.

* Do great-looking tests.

* `Event places`, not `events places`. color-space, not color-spaces. moduleId, not moduleIds. Get rid of plurals - it’s an extra clutter.

* Use .npmignore or "files":[] to exclude shit, the way as .gitignore.

* Function arguments should be human interpretable. Ideally - answer natural questions. transform(what? how?), buildUp(fromWhat? where? how?).
	✘ `renderRange(color, channel, min, max, buffer, calc)` - too unfriendly.
	✔ `renderRange(color, buffer, options)` - very humanized.

* Write your ideal code, consider external libs as polyfills - tools providing your ideal code. Write tools as polyfills.

* Do not require extra than needed. Use submodules, not modules.

* Do readmes as if you do it for yourself in future - exhaustively. Not for a stupid imaginary user.

* More fragmentation and separation - better. Practice shows that you may need absolutely any method anytime, so having fat grouped libraries instead of atomic functions is stupid. Even if you think it’s not (case of `parenthesis` pkg, where parsing separated from stringifying).

* Also it is good to cover every possible use case. `require(a/b)`? ok. `require(a).b`? ok. `require(a)(arg [, b])` ? ok. `window.b?` ok. `jQuery.b?` ok... But module shouldn’t target them all, it’s a task of builder.

* Simple measure for code quality: imagine your lib as a part of js, on MDN and polyfill. If it is not - it isn’t needed.
	* Color - ok
	* StateMachine - ok
	* Graph - ok
	* st8 - bad, State - good.

* Use autopolyfiller for complex apps, do not include polyfills in modules: you will bundle a bunch of useless duplicated code. It’s not the task of components to polyfill environment.

* An easy way to choose the name for a thing: make it verbose, make it one-word, make it reflect the thing beneath.
	* FIXME: Example?

* JS has two options of writing code: do it concise or do it verbose. Verbose code is able to read by anyone.

* Don’t make work for user or environment. Concisely cover an abstraction and nothing more. Do exactly the thing user can’t do himself. Like, if you distribute draggable, provide only draggable class. Don’t parse element’s attributes - user can always do it himself; don’t autoinit draggables - user can define where to do it better himself. Otherwise you withraw him levels of flexibility, plugin scheme becomes rigid.
	* Don’t deliver environment wrappers, jQuery wrapper, web-component wrapper within your code. Deliver them separately, if really needed.

* Don’t use emmy for components: you already know the way you have to bind events.
	* Don’t use generalized stuff.
	* Thought it’s difficult to remember how to invoke emit.

* It is not really important how to call a method, it is just a reflection of sense. Important is properly designed structure of a module, the meaning of that method, separation and independence from other parts.

* Periodically clean API and options. Separation and focus is better than stacking all together.

* Be courteous to user settings, keep API consistent. If user has passed array as a value, don’t cast it to plain value, learn how to work with that instead. Don’t try to change the world - be wiser instead and listen, don’t expect.

* Don’t invent flags to prevent recursions. Separate recusrion flow instead and control it where needed. Using flag is like covering a symptom - a wrapper to an issue.
	* A good practice is to separate user input from programmatic call, like `input` and `change` events. Not the only `change` event.

* API design compromise: .setN(), .getN() - for explicit convenient old environments, .n - for modern get/set environments.

* How to avoid name aliases? Provide module, then the way user names it on require stage is defined by him.

* Practice collection lib, like 101 but with docs of awesome-*. Docs are done in awesome-* format, but each module is provided as require. Example - color-manipulate.

* Don’t repeat mistakes of others only for API compatibility. Like color.js. Bad design decisions should be folded and cleared, not kept due to compatibility.

* Ideal readme documentation is code. It’s far simpler to copy-paste a part of code with options than to go through custom style of docs. Compare piano-keyboard and audio-stats.

* Init components through options and in no other way, like fromJSON etc. There should no be secret door to do the same thing. Keep API monotone.

* Do not make things which do not bring value to this world.
	* Value brings *new* things (innovations), thoroughly thought well-done things (not new qualitative ones) and things of fun (love, experiments, knowledge-expanding).

* Use multiple name declination for list methods: `this.texture` hints that we access the texture property, `this.textures` hints that we access list. Second is better.

* Think from the user’s POV
	* Usually designers write better docs that programmers as they get in other’s boots

* Commit yourelf to only one project at time, and complete all it’s qualitative sides.
	* When community’s grown - you can leave it.

* Be competent - keep things tidy, error-safe, completed. Do not let laziness a way. Be diligent in cleaning bugs.

* Product vision is everything. Imagine the ideal image of your product, otherwise you'll face troubles in making decisions.

* Throw out stereotypes and common (even scientific) concepts. Try to rethink things from the beginning, as if you’re a monkey. E. g. color picker: using color schemes is good, but naturally, every color has a meaning and application, like paints for the walls, so contiguous space isn’t really needed.

* Don’t expose broken things, links on broken demos. It spoils the positive effect. Value the attention of visitors.

* Stop reflexing, do things. Reflexing & narcissism stops you. You can do more if you dive into the world of things, not yourself with you presentation in twitter etc. Strive to be yourself 5y after, not a someone trying be good-looking. You can do not 2, but 5 packages a day, as well as fix serious task a day.

* Compose polite & firm readmes. No one likes aggressive and uncompetent texts. You’ve to be courteous and polite.

* For serious projects do a scientific article. Do project in a scientific approach.

* If you has a competitory plugins/components, especially open-sourced, don’t compete with them, contribute to them. You’re unlikely to support all the bunch of your plugins better than the open-sourced popular lib, it’s all your pride, overcome it. You harm the community that way. It will be really of a use.
	* Consider analogous projects as a friends rather than competitors - they’ve gone the same way, they have similar foundings. They can get an experience to learn from.


* Object/functional pattern of comonents, like dialog does:
	* var d = draggable(el).within(container).axis('x');
	* var d = new Draggable(el, options);

* Don’t spawn myriad of classes based on type of a thing: PolarPicker, RectPicker, ...., NWidget, ConcertWidget, ... Empirically it inflicts pain in support. Better use stateful polymorphosm - it is very concise and coherent. type: {rect: {}, polar:{}, ...}

* Don’t use fear-driven approaches like we won’t have enough space, we will die and therefore need a community etc. Use peaceful eternal approach - github can always open access to the well-known repo of a dead man.

* I should not try to prove anything to anyone or try to like to anyone. Things have to be kept functionally, as they are. No need to create marketologist descriptions in readme or explanations of obvious.

* I don't need loud exclamations, I need modest and qualitative site. (W3C schools compared to MDN)

* OBSOLETE: Release at the end of the day - any price.
	* Why? That way you risk deploying broken stuff.

* Function should throw error only in case if data is really something weird - when it helps user catch a bug. Otherwise, especially if data makes any minimal sense - let it passed. array-flatten case, which forced input be strictly array, not typedarray.

* Tutorials should not contain non-API code, unless it is really lucky. Otherwise user can decide for himself how to group things.

* Create minimal working version - the best known solution. Do not target on covering all possible use-cases, target one use-case which you will probably reuse with pleasure many many times, not the multitude of use-cases.

* Generally tools which stand between user and API are flawed. choo, react, jquery, polymer ...

* Do not create shallow options, each option has to have a value. Neither create purely visual options - they are not required. Bad options: mask, group, snap, balance (in sense of only visual setting). Good options: min, max (for visual contrast inc.), type (for style of rendering).

* Require thing only in case if there are 3+ use-cases and thing size - 20% will be less than current number of uses.
	* Do not require if you are just used to it, like jquery. Get used to API, not wrappers.
	* Requirements binds you to version control, new bugs and

* offset/width is better than start/end, because it is abs/relative vs abs/abs, more info is brought, easier calculations.
	* offset/scale is even better, because it relates back to the absolute grid, you can get rid of ratio param

* semantic layout is not always possible nor make sense. Imagine some pointilism picture presented in HTML, or calligraphy with lots of swashes. You can give classes `swash-1`, `swash-2`, ... or even `swash-y-1`, `swash-k--top` etc, but reality does not name things. Better approach is to group inner elements into a larger block and put styles linline or in tachyons style.

* use NullObject convention

* create trust between components. Let children modify the container - samsara. No need for asking permissions. If destruction happened - unroll samsara by changes.

* only excuse for async API is potential idle processor time, like threads/networking/app devices. If a function has sync argument, like image.src = data:image, that is leaking API

* Do not generalize code in tests. Every test case should be recreated from scratch, because it is at the same time the use-case.

* If a package includes dependency significantly larger than the package itself, it should have it as a prefix

* If multiple possible solutions are possible, do not create a solution implementing all of them, unless that is naturally the result/solution: that will have the most expensive maintenance. Do not pick any solution either: that is risky to be more expensive on refactoring. Try to do a good guess, then you will pay exactly the price of your mistake - your experience, or knowledge.

* Do not solve issues that can be easily solved with language syntax. Eg. do not return Builtins extended with properties: {data, w, h} is better than <Uint8 data, w, h, ...>.
	* JSON does not stringifies that
	* let {data, w, h} = do() is easily solved by lang, no need in let data = do()

* Fail loudly when further processing is dangerous, eg. broken data. Fail silently when further processing is allowable.
	* Merges in detecting validity of data: null result indicates invalid data, but not failure

* Methods should follow in the order of invocation the component in the real use-case

* Abstract away from data structures, work on the level of logic. For example, if you parse some syntax - it doesn't matter what structure is used for describing some token - an object, string, symbol or array. Logically that should act the same - comparison, replacement etc - don't waste energy paying attention to language peculiarities, that's the 3rd stage question.
	* That comes to migrations too - if you upgrade structure from string to object, it shouldn't change program logic. TDD in that case is just a tip of iceberg, the core is basic structures and operations with them.

* Make it right: separate model entities (emmy example), do not mess spaghettie style in methods

* Create projects as if it's going to receive 0 stars in github
