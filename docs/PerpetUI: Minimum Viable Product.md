# PerpetUI: Minimum Viable Product

The frameworkless framework for declarative web components.

Instead of a framework, PerpetUI offers a library of highly decoupled abstractions that can enrich both the development and depolyment of web components.

## Goal

Compared to other frameworks, PerpetUI has a very humble goal, to uncover and encourage design patterns that empower frameworkless declarative web components with clean, open and easy to learn from code by using the builting inspector.

- No, your components will *not **not*** have dependencies, they absolutely should if they absolutely need them. Yes, they can work without them if they don't need them.

- No, your components will **not** need bundling and can simply be statically or dynamically imported imperatively and declaratively. Yes, they can still be bundled when imported just like any ES import, and while some workflows can struggle with your component's resources like styles and graphic elements, PerpetUI's design philosophy should help mitigate some of those complexities.

- No, your components will **not** be more efficiently loaded when bundled, especially if they are poorly bundled. Yes, the browsers can and will optimize and cache your components a lot more efficiently than any and all bundlers ever could, especially if they are served right and when service workers (if used) are setup accordingly.

- No, frameworkless does **not** mean your components must be baked (aka transformed using some secret ingredents) to be production-ready, PerpetUI's philosophy is to ensures that your HTML, CSS and JavaScript is written exactly as it is, without any hidden magic. Yes, they could theoretically be baked but this is neither encouraged nor planned to be implemented.

## Minimal Viable Product

The initial release will make it possible to use Tagged Templates like `html` and `css` to create HTMLTemplateElements and HTMLStyleElements optimized for custom elements on supporting browsers, with strong emphasis on predictable and traceable declarative syntax, with minimal boilerplate and maximum throughput.

The design of the templating prototypes will assume a more generalized concept of an "abstract" view for which HTML is one realization, without any intentional effort to a formalized path for any other realizations, aside from simple stringification.

The design of the caching prototypes will be limited to natively consumable URI's, without any bundlers or loaders, aside from those achievable using platform features like ServiceWorkers or Experimental Loaders (in Node).

While the MVP is expected to function on WebKit and Firefox, it is going to be optimized for Chromium and will target the browser (ie Chrome) and apps (ie Electron and NW.js). It should still be viable (with some effort) to target other browsers, for both desktop and mobile, for experimental deployments only.

### Aspect 1: Templates

The tagged templates specifications provide a highly optimized variable string concatenation mechanism. This mechamism carries some initial overhead each time a new tagged template expression is evaluated, ie the invariant string parts of the expression change. Libraries that use tagged templates to generate custom productions (like elements) often capitalize on this mechanism and also "wire" their templates for new tagged template expressions then quickly instantiate or "clone" their templates on "render". PerpetUI does not follow that pattern.

Instead, PerpetUI is oriented more towards Web Components and follows a more decoupled architecture from the inherent string concatenation optimizations, allowing engines to perform such optimizations independently from element optimizations, which take place mostly within the scope of HTMLTemplateElement.

As far as Web Components specifications are concerned, conceptually, custom elements are specific things with finite behaviours, like buttons, button groups, or dialogs with button groups that have buttons, so the fact that a library optimizes the template of a button once happens naturally through element composition (ie by the browser) instead of forcing it to occur somewhere between stringification and concatenation, which would be extremely taxing at such a low level — for each and every element.

At a higher level, the "wire" and "render" pattern is an absolutely great one if and when it is used correctly. This is where PerpetUI yields it's influence to let your application frameworks and libraries (or not) do what they do.

### Aspect 2: Reactions

Obviously web components are inconcievable without states, however, not all states are equal nor are they be created equally. PerpetUI considers two distrinct categories for state, reactive and imperative. Imperative state is where the application logic lives, whereas reactive state is a split between presentation and user interaction, ie handling of life-cycle and input events.

Building on top of emerging patterns, like Fred Dauod's Meiosis pattern, PerpetUI will aim for almost absolute decoupling from impertive state aspects through the Async Iterator protocol and/or update callbacks, allowing it to integrate with virtually any "action" oriented state management library.

Reactive state will be handled with a more concrete API centered around the notion of "reactions" which unfold in a predictable fasion that can be modeled the aggregation of one or more finite state-machine generators. Simply put, a "reaction" reflects the transition from one finite state to another, as a result of a trigger, for which the new state can be of one or more potential states with explicit and distinct preconditions. In this context, triggers can include life-cycle event, user interaction, or dynamic updates.

### Aspect 3: Resources

The most essential resource that is inseparable from any "concerete" custom element (ie UI building blocks) is styles. Because, in most cases, script resources are anti-patterns, and stylistic graphics can either be inlined into the "default" styles or picked up from the "theming" stylesheet which is loaded externally. If this is confusing, and it should, then read on.

Essentially, when a custom element is upgraded, two things should take place, a new structure is rendered for the element, and, custom behaviour is attached to features within this new structure. Because upgrades take place within the context of a web page (ie HTMLDocument), one can assume that this page can and likely already links to "theming" stylesheets and other dependencies like scripts. In this case, a custom element should only load resources that are scoped to it's own shadow root, and do so without redundancy, so browsers can avoid extraneous network or memory overhead.

If a custom element depends on libraries, they must do so without making assumptions on how they are imported or loaded and use some declarative linking abstractions that statically indicate the library resources so that browsers can optimize accordingly.

While ECMAScript modules offer the perfect abstraction for this with static imports, it is still not possible to use node-style or "bare" specifiers like `import x from "awesome"` because there is still no consensus on how browsers should resolve such specifiers. Instead, browsers expect URLs which can be absolute or relative — ones that start with "./", "/" or "//" which resolve relative to the URL of the importing module or the baseURI for the importing element.

    TODO: Try `<script type=module base=…>import x from "/awesome"</script>`
    TODO: Try `<style base=…>@import "/awesome"</style>`


 Structure here refers to both layout, size, style and some content.

## Non-Goals

- No intent to support browsers that are behind.

  Platform support for web components is around the corner for the most widely adopted mobile and desktop browsers, so instead of backwards compatibility, we want those browers to catch up. Deploying on legacy browsers is a choice that is not considered by PerpetUI, not because it cannot be done, but simply due to the wide range of decisions which should not be limited during design and prototyping.

- No intent to design around polyfills or ponyfills.

  In most cases, a component built to work on a supporting browser will be functional with polyfills on legacy ones. Often times it takes at least two vendor implementations until a new specification becomes reliable enough from a component design standpoint.

- No intent to design around bundlers and custom loaders.

  Let's face it, bundlers can be used right to solve a lot of problems on the web. But the reality is that this eco-system has been flooded with opinionated (and highly non-conforming) templates powered by a façade of magical powers that take away any room for sane decision-making. So bundling for production is not discouraged, but magic production apps without proper source-maps when you are using 100% open source code simply takes away from new generations the benefits of learning that has made it possible for us to mutually learn and shape today's amazing web.
