# Architecting for Frameworks, not Websites

* When authoring the CSS for an enduring project, the technology used to produce the CSS should be largely immaterial. [Sass, LESS, Stylus, etc]
* “I consider a pre-processor for style sheet authoring essential. This allows a differentiation between ‘authoring’ style sheets (the style sheets that the author writes in their pre-processor of choice) and the ‘resultant’ style sheets (the compiled and minified CSS that gets served to the user).” Ben Frain
* Requirements of a preprocessor
	•	variables (to mitigate human errors with colour picking and specifying constants like grid measures)
	•	fragment/partials of styles (to facilitate one-to-one parity with a feature branch or template)
	•	basic mathematic operations (to negate reliance on ‘magic’ numbers)
	•	color manipulations (to allow consistent manipulation of the aforementioned variables)
* Build Tooling: “A build system must also be available that can run automated ‘house keeping’ on authored style sheets. The human factor should be eliminated wherever possible and tools employed to do mundane tasks more accurately.” Ben Frain
* Currently, popular thinking tends to place significant emphasis on writing DRY code, abstracting styles as much as possible and having the smallest possible CSS codebase with which to work. My own approaches are, in some cases, at odds with some of those popular principals. “Typically, my foremost goal when writing CSS for enduring and rapidly changing web application is long-term maintainability. As a concrete example; being able to delete an entire Sass partial (say 9KB) in six months time with impunity (in that I know what will and won’t be affected by the removal) is far more valuable to me than a 1KB saving enjoyed because I re-used or extended some vague abstracted styles.” Ben Frain

* Flat selectors
	•	Use only classes for selectors except in specific circumstances
	•	Never nest selectors unless essential
	•	Complete avoidance of IDs as styling hooks

* Namespacing
	•	BEM or module prefixing
“As name-spaced components are almost guaranteed to not leak into other components it makes it incredibly easy to build out and iterate on new components as new design requests are received. It affords a hitherto un-afforded blanket of impunity. In some ways it is the opposite of DRY; we are gaining a degree of component isolation at the cost of verbosity.”


* Ability to deprecate/CSS Bloat
“In the situation of a rapidly changing and enduring project, where many different hands have, and will, be on the CSS, my empirical findings have been that more CSS bloat accumulates due to author deletion fear (“I don’t know what that’s for so just leave it in”) than duplicated styles.
It’s likely that CSS authors are more likely to simply add to existing CSS than risk removing huge swathes of it because it’s difficult to be entirely sure what will be affected.
Consider an example. Suppose a component of a web application is deprecated, and you’d like to remove the associated code (we will cover how this scenario relates to templates and any associated JS in a moment) without affecting anything else. This usually poses some difficult questions: where are the CSS rules that are no longer needed? One file, many files? It’s often a case of going back to relying on ‘find in project’ in the text editor. And even then, how can you be sure nothing else is using anything defined in those styles?”

* Sass files set up for modularity/DRY, CSS set up per-page to reduce size to the browser; possible separate “global.css” for cached, shared components on all pages.

* Naming with intent, human-readable classnames

* Detailed inline documentation

* using plugins vs rolling your own?
	•	Author wherever possible in W3C compliant CSS code. This makes the authoring styles more portable if you decide to switch pre-processor or copy code to a new project and reduces dependency (as a bonus, particularly if using Sass, it can also significantly reduce compile time).
	•	…but don’t reinvent the wheel. In my experience, most grid systems, esp ones that use classes in HTML, are too complex and too difficult to override for my needs. So rolling my own simple grid mixin or using something like Susy is easier for me. BUT, that may not be the case for your project.

* Markup in CSS files for style guide generation
“Comments are good. Indeed, writing lots of meaningful comments is encouraged. However, refrain from entering examples of intended markup in the comments of style sheets. They immediately create a scenario where information is likely to become obsolete and confusing. Instead, the template stub that sits alongside the style sheet should provide all the markup. As this template is necessary for the application to function, it is by nature always up to date and provides a more meaningful reference.“

* Look at your output!
	•	nested styles in placeholder extends = lots of CSS
	•	placeholders styles get added where you write the extend, not where you reference it.

* The cascade is your friend
	•	tag inheritance “We use HTML tags to add semantic meaning to our sites. When we apply styles to these tags we end up mixing semantics and presentation. I don’t mind applying a very basic set of default styles to our tags, but we ran into problems in our example when we tied our finished, presentational styles to the h2 tag.”
	•	location inheritance “This type of thinking goes completely against the mantra of components should be standalone, reusable, and have no knowledge of their parent container. Using location inheritance means that your header styles are dependent on being in the sidebar (not standalone or reusable), and that you’re required have intimate knowledge of your parent container before writing styles.
”
	•	class inheritance. “Class inheritance is the practice of applying an entire system of styles on a collection of markup via a single class. Imagine that you wrote the .block code, and then someone else on the team decided to create .my-calendar-widget using .block as a set of base styles, i.e. a “relied upon module”.Unless you have a well established system for documenting these relationships, this becomes what I like to call “hijacked inheritance”. We have no indication in our CSS that .block has another component tightly coupled to it, so we’ll be dealing with random bugs on our calendar page every time we update .block.”
	•	Composition is the idea that an html component might “contain” other styles, rather than “inheriting” those styles. You can see this principle used extensively in OOCSS, BEM and Sass @extends. Instead of layers of styles, each overriding the last, these approaches embrace the Single Responsibility Principle with hyper targeted selectors that do one job well and one job only.


## Resources

* http://benfrain.com/enduring-css-writing-style-sheets-rapidly-changing-long-lived-projects
* http://www.phase2technology.com/blog/used-and-abused-css-inheritance-and-our-misuse-of-the-cascade/