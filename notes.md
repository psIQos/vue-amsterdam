 # VueJs Amsterdam



[TOC]


## Day 1 (Thu, 20th Feb 2020)

### Talk 1 (8.45 - 10.15) - **State of the Vuenion** (Evan You)
- rocky start with half-standing ovation
- Vue 3 is apparently really close ("pretty much achieved all the goals", "getting ready for release")
- 1.1m weekly active users (Vue devtools)
- 5.45m npm downloads
- Vue 3 development (beta very soon ~couple weeks)
+ final release in Q1 not probable (still important work to do)
+ vue next is already available Vue 3
+ Late '18 - early '19 experimental phase
	- typescript as first class citizen
		+ type definition leverageable in IDEs even for plain javascript
		+ 'It just works.'
	- jest for testing
	- monorepo architecture
+ Early - Mid '19
	- faster than Vue 2
	- renderer rewrote from scratch
	- composition API (inspired by react hooks)
+ Renderer Runtime improvements
	- bottleneck of virtual-dom updating is updating large structures of the dom
	- performance is not based only on the dynamic content but rather on whole size of the template
	- except for v-if and v-for the tree structure is static - so the tree structure is known and can be leveraged to only update the contents of the dom that need updating
	- new rendering scales directly with size of dynamic bindings!
+ Late '19 - now
	- Composition API done
	- still working on feature-completion
	- easy migration guide is being worked on and will be published
	- server-side-rendering?
		+ basic support is already in alpha
		+ as long as no components are included sole string-concatenation can be utilized
		+ up to 3 times faster than vue2 ssr
		+ ssr with components is realized through monadic structures (a component containing a component and string-based-templating is utilized)
	- close to beta!
	- new compiler
		+ full source map support
		+ numerous optimizations
		+ better static tree-hoisting
			- static content is hoisted and not considered during dynamic updates
		+ event handler-caching
			- handlers for dynamic nodes are only created once and then cached
		+ demo available at: https://vue-next-template-explorer.netlify.com (automatically compiled against the latest version)
	- ssr hydration (for string-based and component based structures)
		+ static content is not rendered again (true also for components)
		+ this also results in very lean bundles after tree-shaking
		+ ssr is not fully completed (suspense support not ready)
- a lot of first time supporters in PR's
- mixins will be replaced by Composition-Api

 #### Review:
- good talker, lots of interesting stuff
- perfect slides

 ### Talk 2 (10.15 - 10.40) - **Scalable Vue graphics for the web** (Dima Vishnevetsky)
- 92% used svg before (according to Dima)
- svg has great potential beyond simple icons and graphics
- vue-svg-loader as tools for support or just plain svg files in components
- svg is often simpler and easier than html+css for animations (duh!)
- svg can be used for 3d animations (zlog)
- micro interactions (animations for underline of inputs)
- *logo animations*
- svg enables mouse interactions based on svg-paths (not bounding box)
- dynamic interactions with images and backgrounds
	+ overlay of graphic on image with dynamic changes
	+ svg effects on text
- svg benefits:
	+ vector-graphics (duh)
	+ file size can be very small
	+ animatable
	+ syntax -> readable, changeable
	+ supported in almost all browsers
	+ seo
- link to codepens will be tweeted:

 #### Review:
- not a lot of new information
- contained uninformational / more inspirational content

### Talk 3 (10.40 - 11.05) - **Vue / Test utils in 2020** (Jessica Sachs)
- what is vue test utils (vtu)
	+ wrapper for components as abstraction for testing vue components
	+ wrapper.vm for accessing the vue instance in case the wrapper doesn't provide what you need
	+ Vue Testing Library (selenium  near) builds on top of vtu
	+ lean core experience with a plugin architecture
	+ setProps is a complicated method that requires hacky work
		- setMethods, setData, setProps can have **unintended consequences in production**
		- could appear to be covered but immediate watchers are apparently a problem - if not correctly set it can result in different state in production
- new maintainers
- guiding principles for vue testutils
	+ support for Vue 2 & 3, Composition-Api
	+ official guides for upgrading test suite
	+ upgrading is planned to be incremental
	+ VTU is written in Flow
	+ IDE-support for types
	+ better documentation is big focus
		- examples vor i18n,  native events, custom events
		- best practice section
- vue 3 and the v1 Release
	+ vue test utils is still in beta
- ```shallowMount``` versus ```mount```
	+ ```shallowMount``` -> ```mount({ shallow: true })``` could be changed in the api

 #### Review
- hard-to-read slides
- pretty unstructured talk (points they wanted to touch on were missed)

------------


 ### Talk 4 (11.40 - 12.05) - **Vue 3 Reactivity** (Gregg Pollack)
- Why its needed / limitations in Vue 2
	+ readability with growing component structure
	+ logical concerns are organized by component options (computed, methods, props, data, ...)
	+ organized by logical concerns
		- with composition api
			+ setup method that contains logical structure
			+ with composition functions ("useSearch", "useSorting") that are called in setup functions
	+ extracting functionality
		- mixins: organized by feature
		- mixin factories: namespaced mixins
		- scoped slots: organized by functionality in different components
			+ less performant
			+ properties only accessible in the template
		- composition functions
			+ called from setup
			+ exposes whats needed
			+ configuration in comp functions
			+ flexible
	+ when to use composition api:
		- typescript support
		- large components
		- reusable components / plugins
- setup functionality
	+ executed early in lifecycle (no access to ```this```)
	+ instead ```context```
	+ reactive reference:
		- wraps a primitive in an object
		- declare reactive reference: ```const capacity = ref(3);```
		- explicitly specify exposure: ```return { capacity };```
		- already available as plugin for vue 2
		- reactive reference is an object - not a primitve (access via ```object.value```)
		- inside the templates references are automatically exposed (no need for ```.value```)
		- ```computed``` - declare as function returning the computed value in setup function
		- different syntax for declaring reative references (```reactive```)
		- destructuring object removes reactivity
			+ **reactivity in Vue 3 is bound to the object**
			+ destructuring using ```toRefs()``` (keeps references)
		- usage: move setup logic from setup to composition function
			+ when in different files just import the function (because it returns the exposition)
			+ reactive references that need to be exposed can be destructured in return of setup function
- reactive references
- methods

#### Review
- extremely good talker
- very good code examples
- structured examples and walkthrough
- maybe consider subscription to vuemastery

###  Talk 5 (12.05 - 12.35) - **Composition-Api emerging best practices** (Thorsten Lünborg)
- composition-Api is already used by libraries
- ```ref()```vs ```reactive()```
	+ ```ref()```is used by the vast majority
	+ ```ref()``` has a greater consistency
	+ *sometimes* ```reactive()``` doesn't work
		- computed properties are refs
		- dom references requires ```ref```
- setup functions mimic interfaces in the usage in a way
- handling refs in arguments
	+ ```isRef()``` for checking arguments (exposed from vue)
	+ accept refs and static values
		- wrap in ```ref()```
		- declare argument as ```el: Ref<Element> | Element```
		- wrap with ```element = wrap(el as Element)``` (also possible w/o typescript)
- watch vs lifecycle hooks
	+ ```watch(element, (el, _, onCleanup))```
	+ addEventListener only when watch receives a receives something (no null init)
	+ onCleanup removes the eventListener when the element disappears
- returning refs instead of computed means that reference could be destroyed
	+ ```readonly(Object)```
	+ ```computed(() => {ref.value} )```
- naming conventions:
	+ in composition functions use names that makes more sense in the context
		- ```const fullscreen = fullscreenCompFunction()```
		- ```fullscreen.enterFullscreen``` vs ```fullscreen.enterFullscreen```

#### Review
- very good examples
- very instructive
- good talker

### Talk 6 (12.35 - 13.05) - **Vue storefront and Composition-Api** (Filip Rakowski)
- vue storefront is the most popular frontend eCommerce framework
- backend agnosticity using elasticsearch and transforming backend data
- mixins have no communication about what hey are mixing in (what they are exposing)
	+ Composition-Api is more flexible in this way
	+ consumers can just use what they need
	+ easier to introduce non-breaking changes
- Composition-Api can in part replace vuex or other state management
	+ variables in a composition function are a quasi-"shadow"-state
	+ *but* needed parts can be exposed - even as immutable/readonly values
- storefront ui (see Talk ##XX)
	+ component library
- powerful cli

#### Review
 - good structure
 - more of an announcement talk (similar to Talk ##1)
 - could be interesting to apply to store

---------------

### Talk 7 (14.20 - 15.00) - **Serverless static sites with Vue & NodeJs** (Roman Kuba)
- serverless obviously still needs servers
- easy to implement and execute pipeline is key
- "Fauna"
	+ platform built for serverless with GraphQL


​		
#### Review
- weird explanation of lambda functions
- not a lot of information


### Talk 8 (15.00 - 15.20) - **Climate change and the tech community** (Callum Macrae)
- methods to reduce impact on global warming:
	+ solar radiation management (reflect/absorb uv-radiation from the stratosphere)
	+ co2 storing
- ipcc policy banker report (~20 pages)
- 1% of global power usage comes from data centers
- 1.1% comes from data transmission networks
- combined "the internet" has a higher power consumption than almost all except for 9 countries
- methods to reduce app footprint
	+ lazy-loading
	+ svg graphics
	+ reduce size
	+ more efficient code
	+ utilize caching
	+ green energy
- *a greener website is also a faster website*
	+ faster loading
	+ better user experience
- measure carbon footprint:
	+ https://www.websitecarbon.com
	+ frontenddeveloperlove ~ 8g co2
- google lighthouse to analyze pages
	+ lots of helpful suggestions for improvements
- https://climateaction.tech

#### Review
- well researched
- explored different angles and tools to use


### Talk 9 (15.20 - 15.45) - **Handling Global events with renderless components** (Rolf Haug)
*application shortcuts with a renderless event component*
- renderless component:
	+ empty render function / empty template
- assign event as prop ```event``` and add event to ```window.addEventListener()```
- ```destroyed``` lifecycle hook should remove the event listener
- can also listen to network events
- ```@fired``` to receive event from renderless component
- ```this.$listeners``` gets all assigned listeners from a component
	+ iterate over the event list and add them to global event handlers
	+ ```destroyed``` remove them afterwards
- can also fire different events: ```@offline```, ```@online```
- shortcuts need to be communicated well to users
- companion repo: https://github.com/vueschool/application-shortcuts
- Package for global events: vue-global-events

#### Review
- very informative
- instructive examples
- good talker

----------------

### Talk 9 (16.30 - 16.50) - **Content loading that isn't broken** (Maria Lamardo)
- web accessabilty
	+ communicate on multiple channels (i.e. not on color information alone)
- structure
	+ heading
- routing
	+ screen reader doesn't anounce client-side javascript route change
	+ aria live reader
	+ "what we learned from user testing of accessible client-side ..."
	+ meta tags to rotes (*how to do this with nuxt-routing?*)
- aria-live regions:
	+ ```assertive``` announcements that interrupt user flow
	+ ```polite``` announcement when user is idle
	+ ```aria-current``` indicate a current item on a set
		- ```page```, ```step```, ... different items for different content
	+ browsers handle voice-over differently, also screen-reader have differing compatibility with browsers
		- nvda (firefox)
		- jaws (ie)
		- voiceover (safari)
- tabIndexing
	+ indicates focus property of an element and focus order
	+ index of 0 is accessible in order of dom
	+ index > 0 is acessible after regular dom order
		- should **not** be used
		- because it is very confusing to the user
	+ index -1
		+ programmatical access via ```focus()```
		+ not accessible through normal tabbing
- accessible naming:
	+ ```aria-label``` good for labeling elements that have no displayed names
	+ ```aria-labelledby``` establishes relationships between objects
	+ ```aria-describedby```
- ```v-focus```-Directive
	+ when directive is present focus element

#### Review
- good examples
- 

### Talk 10 (16.50 - 17.20) - **Team First** (Tim Benniks)
- framework for ensuring team success in a high-pressure environment
- behavioural rules:
	+ as a leader
		- make the team feel responsible for their work
			+ trust 
			+ overview
				- everybody needs to know what all disciplines do
			+ ensure growth
	+ as a team member

#### Review


### Talk 11 (17.20 - 17.45) - **What is Nuxt 2020** (Sebastien Chopin)
- automatic code splitting for pages
- resource hints + http2-support
- ```nuxt build -a``` to analyze bundles
- Server side rendering and static generation
- ```ssr: true | false``` replaces ```universal``` / ```spa``` mode
- ```fetch()```
	+ new ```fetch``` has access to ```this```
	+ global access
	+ ```fetchOnServer```
		- switch between fetching on server or client-side-only
	+ works with full static generation
	+ ```$fetchState.pending``` on new route
	+ **basically normal vue behaviour with ssr**
- ```target```
	+ ```target: server``` for nodeJs hosting
	+ ```target: static```
		- for static hosting
			+ different ```context``` props exposed on ```asyncData``` etc.
			+ crawler generates dynamic routes
				- depends on ```<nuxt-link>```
			+ server side payload is saved (as ```.json``` file)
				- ```static: false``` to disable saving payload on ```asyncData```
		- also supports serverless mode
	+ not released yet (eta: 3-4/'20)
- Nuxt 3 release should be ready when Vue 3 is released

#### Review
- good talker
- interesting new points and tricks


​	
### Talk 12 (17.45 - 18.15) - **Deep Dive into Nuxt.js** (Pooya Parsa)
- Nuxt.Js is very modular
	+ internals are fully modular
	+ framework behaviour can be modified through plugins
- nuxt functions
	+ higher level ```serverMiddleware```
	+ ```functions/``` directory (like ```pages/```)
	+ ```$functions``` service at runtime
		- e.g. ```$functions.auth.getUser()``` directly in component
		- server side: direct function call
		- client side: requests
		- integration of numerous services very easy with functions
- nuxt lambda
	+ super optimized ssr-only-buld



--------------------------



## Day 2 (Thu, 20th Feb 2020)

### Talk 1 (9.00 - 09.35) - **An Animated Guide to Vue 3 Reactivity and Internals** (Sarah Drasner)
- how reactivity works
	+ detect changes -> track what needs change (the *effect*) -> trigger changes
- Proxies:
	+ object that encases other objects (sets getters and setters to the actual object)
	+ ```Reflect.get(...arguments)``` binds ```this```
	+ ```track(target, key)``` - track changes (for getter)
	+ ```triger(target, key)``` - save changes (for setter)
	+ ```data``` properties aretransformed to ```Proxy```s
	+ reactive props are kept track of in a ```Map()``` and the component subscribes to track changes
	+ components are held in a ```weakMap()``` to delete and garbage collect their references when not needed anymore
- DOM updates
	+ virtual DOM to keep track of dom changes
	+ changes are made in batches (singular updates would be inefficient because finding specific nodes is slow)
	+ connection to *Hydration*
		- *Jamstack* to improve performance and security (first request handled by server, subsequent requests answered from CDN's)
		- in short: make a static dom dynamic
- https://github.ocm/sdras/animated-guide-to-vue3

#### Review
- very good visual approach


### Talk 2 (09.35 - 10.05) - **Vuelidate version for Vue 3.0 / Validations in Composition Age** (Damian Dulisz)
- *vee-validate*
	+ template-based validation
- *vuelidate*
	+ model-based validation
	+ a lot of current problems (no lazy support, mediocre async support, no support for error messages, model has to be declared up-front)
	+ -> new Version released (v2.0) build with composition-Api for Vue 3
- new version supports lazy loading
	+ per-validation support of eager validation with ```$autodirty = true```
	+ trigger validation with ```$touch```
	+ *dirty* (lazy-loading) can also be disabled for whole validation
- ```@vuelidate/validators``` - package for different validators
	+ ```$validator``` - validator function
	+ ```$message``` - error message etc. (has access to passed ```$params$```, supports *i18n*)
	+ ```$params``` - access to params passed from outside to validator
- ```schema based forms```
	+ not compatible with UI component libraries
	+ Form*Vue*Latte - lightweight schema-validator that supports other validation libraries
- adapters vor UI libraries like *Vuetify* are planned

#### Review
- moderate structure and presentation flow


### Talk 3 (10.05 - 10.25) - **Authentication from Scratch** (Adam Jahr)

#### Review
- questionable suggestions
- very basic introduction

### Talk 4 (10.25 - 10.50) - **Test-Driven Development with Vue.js** (Sarah Dayan)
- write tests before code
	+ no procrastination of testing
	+ no untested code in production
	+ tests document what code should do - not how!
- color phases
	+ red phase - test fails
	+ green phase - quickest code solutionto pass the test
	+ blue test - refactor code such that it's clean and tests are still passed
- fixing bugs is **far** more costly than preventing them

------

### Talk 5 (11.25 - 11.55) - **Vuetify 2+** (John Leider)
- Observer Api directives
- new components
- a11y
	+ button + menu automatically communicates necessary ```aria``` properties
- full typescript support
- *Material studies*
	+ works on material design specifications but doesn't llok like material design (?)
- ```onIntersect``` Api to defer loading
	+ also lazy detachables -> improves loading times
- LTS for 18 months
- progressive images
	+ supported out of the box (```require```)
	+ increases performance
- automatic-tree-shaking: up to 60% decreased bundle size
- Vue CLI Presets to support easy and preconfigured start
	+ cross-plugin interoperation
	+ unit test & storybook generation
- v3.0 is going to use composition-Api
	+ release planned for Q2/Q3
	+ Status at: https://notion.vuetifyjs.com
- base vuetify app creatable via cli -> lighthouse score **100%**


### Talk 6 (11.55 - 12.20) - **New Vue. New Compiler. Let’s unbox** (Rahul Kadyan)
- huge performance benefits
- template compiler
	+ converts templates to render functions
	+ template elements are compiled at buildtime
	+ template property of vue bilder compiles templates at runtime
	+ Parser
		- parses template elements to construct abstract syntax tree (AST)
	+ Transformations
		- transform AST to ```expression```s of Vue Api
		- ```node``` & ```directive``` transforms
	+ CodeGen
		- generates the actual code from the tree structure
- sfc compiler (single file component compiler)
	+ templates is given to template compiler

#### Review
- very technical talk
- score: xxxoo


### Talk 7 (12.20 - 12.40) - **You might not need Vuex** (Natalia Tepluhina)
- shared state is needed for:
	+ independent components that depend on a common state
	+ different routes
	+ nesting
- dependency injection vue via ```provide``` and ```injection```
- Apollo client
	+ works with GraphQL
	+ ```@client``` on fetch to fetch data locally
	+ Apollo + GraphQL stores responses automatically locally in the Apollo cache
	+ apollo has own devtools
- demo repo: https://bit.ly/no-vuex

#### Review
- score: xxxxo

### Talk 8 (12.40 - 13.05) - **Vuex ORM – Relational data management in Vue** (Kia King)
- provides relational access to state data
- instead of multiple calls to api use single call and transform data
	+ treat vuex as database with relational model
	+ recommended read: *normalizing state shape* in redux
	+ it exists in redux -> vuex *has* to have it
- built on top of vuex
- official plugin support for *axios*, *GraphQL*

#### Review
- score: xxxxo

------

### Talk 9 (14.30 - 14.50) - **BootstrapVue - The road to 2.0 and Beyond** (Jacob Müller)
- comprehensive implementation of Bootstrap v4 components and grid system
- lots of information about contributing toopen source projects:
	+ start small (*actually most contributors started with extremely small prs, like fixing single spelling mistakes*)
	+ don't be afraid to contribute
	+ as a maintainer: look after your community and make sure to be inclusive towards new contributors
	+ have patience - if you continually contribute to ap roject in a meaningful way chances are good you're invited as a maintainer

#### Review
- xoooo


### Talk 10 (14.50 - 15.15) - **Micro Frontends: Composing a Greater Whole** (Yoav Yanovski)
- needs:
	+ unified user experience
	+ small bundle size
	+ autonomous 
- open source available micro-frontend orchestrator (mfo): @vonage/micro-frontends

#### Review
- unashamed obtrusive ads
- although somevery interesting concepts
- score: xxooo


### Talk 11 (15.15 - 15.35) - **What you'll love in Vue 3 / High level introduction to Composition-Api** (Alex Kyriakidis)
- no reactivity caveats anymore
- ```Portal``` component with ```target="##someTarget"``` attributes
	+ ```portal-vue``` plugin
- multiple root nodes possible
- ```v-model``` changes to ```v-model:prop```
- Drawbacks of Composition-Api
	+ overhead of introducing ```ref```s
- ```Suspense``` component
	+ with ```<template ##default>``` and ```<template ##fallback>```
- ```filter``` is changed:
	+ support for ```{{ value | filter }}``` is removed
	+ instead: ```{{ filter(value) }}```

#### Review
- score: xxxxo


### Talk 12 (15.35 - 16.05) - **Performant Components through Customization** (Maya Shavin)
- choosing a component library
	+ needs to fill the goals
	+ customizability
	+ ease of use
	+ documentation
	+ (bundle size)
	+ (support)
- lazy-load whenever you can
- developer-first approach
- when needed adopt external optimized solutions

#### Review
- score: xxxoo

------

### Talk 13 (16.50 - 17.10) - **Announcement related to Vue CLI UI** (Guillaume Chau)
- vue-cli can be used with plugins / other frameworks
- mono-repo-support
- **guijs** -> gui for js
- size < 2MB
- command palette
- keyboard shortcuts
- has dark theme
- ... has a terminal ...
- guijs app (built with tauri & rust)
- dev build already available

#### Review
- score: xxxxo


### Talk 14 (17.10 - 17.40) - **vue-router and its techniques** (Eduardo San Martin Morote)
- matcher transforms url to route object and vice versa
- dynamic routing
	+ order is important for route matchers
	+ one solution would be to introduce an order
- router history modes
	+ ```mode: history```
	+ ```history: createWebHistory()```
		- better tree shaking
	+ History Api
		- ```history.pushState(data, title, url)```
			+ push on to the history
			+ enables going back w/o changing history length
		- ```window.addEventListener('popState')```
		- ```history.replaceState(data, title, url)```
		- ```location``` Api: ```domain.com/page?query=search##badguy```
			+ Path: ```page```
			+ Query: ```query=search```
			+ Hash: ```badguy```
- vue-router-next
	+ navigation direction
	+ dynamic routing
	+ better encoding defaults
	+ custom history
	+ better documentation
	+ support for composition-Api
	+ 25% smaller bundle size

#### Review
- score: xxxxo

### Talk 15 (17.40 - 18.10) - **Building Blazing Fast Sites with Gridsome** (Jake Dohm)
- PRPL pattern
	+ push
	+ render
	+ pre-cache
	+ lazy-load
- gridsome provides centralized GraphQL store

#### Review
- score: xxxoo


### Talk 16 (18.10 - ) - **Full Static with Nuxt** (Debbie O'Brien)
- static is also dynamic
	+ through api, cms, ...
- doesn't expose api
- high performance
- spa inside static side with ```fallback: true```
- better offline support
- cheaper, safer

#### Review
- score: xxxxo

--------------


## Other
- ```require(example).promises```
- escape hatches as attributes in tags for later testing, e.g. ```data-text="*"```
- mono-repo
- tauri
- vue storefront
- netlify
- hasura (open source) for instant GraphQL api
- apollo as GraphQL client
