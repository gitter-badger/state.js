## Version 5.4.1
Fix internal transition bug.

Add an internal transition test case.

Remove a litte redundant from the runtime given the refactoring of Choice and Junction pseudp states; a little post 5.4 refactoring.

Added explicit testing of static conditional branch implementation for junction pseudo states.

Addded StateMachine.setError to enable custom handlers for errors; defaults to throwing an exception.

Renamed Element.ancestors to Element.getAncestors in line with other getter methods. This is not supposed to be a part of the public API hence no breaking change.

### Version 5.4.0
NOTE: while the API to state.js has not explicitly changes in this release, the changes made *may* impact the behaviour of your state machines hence the bump to 5.4 indicating a potentially breaking change. See the release notes pertaining to Junction and Choice pseudo states where the message passed to guards and user defined transition effect behaviour will be the same for transitions after the pseudo state as transitions before the pseudo state (previously, the message was the pseudo state itself).

Fixed bug #13 - Junction transitions now properly implement a *static dynamic branch* (guards on transitions before and after the junction pseudo state are evaluated before traversing the transition before the junction pseudo state) rather than a *dynamic conditional branch* (where the transition effect on prior to the pseudo state can be observed in the guards after the pseudo state).

For consistency with Junction, re-implemented the logic for Choice transitions to ensure that the message passed into guards and user supplied behaviour is the triggering message and not the source vertex.

Added a TransitionKind enumeration ready for future support for *local* transitions in addition to *external* transitions.

Add a kind member to Transition which defaults to External (or Internal if no target vertex is supplied in constructor).

Re-worked transition initialisation logic based on transition kind.

Addded StateMachine.setWarning to enable handlers for warnings.

### Version 5.3.6
Fix a minor bug in choice pseudo states; where multiple transitions are found not all are chosen equally.

Fix a bug in terminate pseudo states where isTerminated was not being set when reaching a terminate pseudo state.

Let PseudoStateKind.Initial be the default for the kind parameter in the PseudoState constructor.

Improve testing workflow: use mocha and istanbul; send findings to code climate.

Add tests to improde coverage of untested capability:
* user defined behaviour
* choice pseudo states

Split source into smaller files.

### Version 5.3.5
Minor refactoring based on Code Climate findings.

### Version 5.3.4
Fix versioning errors

### Version 5.3.1
Remove declaration of Behavior as it was only for internal use and was cluttering the exported API.

Remove the version history as it was baggage within releases (esp. within Node.js); this can be recreated from GitHub if required.

Tidy the browser example:
* Pre-load images and jquery to show/hide images
* Direct logging output to the DOM to display state transitions.

Use the target attribute in the script element to define the global object to bind the state.js API to (fall back to fsm if not supplied).

## Version 5.3.0
Fix common.js packaging issue for use in Node.js!

Please use the following files in the following ways:
* src/state.node.js - this is the CommonJS module for use in Node.js; either reference this manually, or if you npm install state.js, this is the target when using require("state.js").
* src/state.web.js - this is a version for use in browsers; all the classes and functions will be available under the fsm object as in earlier v5 versions.

Renamed public method root to getRoot.

Added an option to turn on log messages programatically rather than (un)commenting code.

## Version 5.2.1
Added Behavior interface in place of using Array of Action.

Revert the Evaulator class to a singleton as its stateless and therefore thread-safe.

Minor performance improvements.

Better code comments in Runtime.ts

## Version 5.2.0
Extract the last pieces of the runtime from the model classes to a set of independant functions. This is the cause for a breaking change to the API.

Use public keyword to distinguish public interface; the lack of a protection keyword is used to imply package private.

## Version 5.1.2 / 3
Changed the visitor implementation to accept multiple parameters after the template parameter.

Moved the message evaluation and transition selection / execution to a visitor to free up core model classes.
* note that the intent is to remove all the executable elements of the code from the core model claseses.

Move Vertex.accept to Element.accept.

Refactor isActive and isComplete out of state machine model.

Use built-in array iterating functions where possible.

## Version 5.1.1
Created singletons for bootstrapping rather than static method in StateMachine and Bootstrap classes.

Inline invoke method and remove Dictionary interface.

Break up the source across mutliple files for managability using tsconfig.json to pull them back together into the same state.js output.

## Version 5.1.0
Fixes a bug that would cause completion transitions to fire for composite states that were not complete.

Changed the file management for releases: the latest version will always be lib/state.js, lib/state.min.js, and lib/state.d.ts; files with version numbers will also be available in lib/versions.

Renamed IContext to IActiveStateConfiguration, Context to StateMachineInstance.

Added an optional name parameter to the StateMachineInstance constructor and a toString method to return that name.

Cache the fully qualified element name during the bootstrap process (as its used in the StateMachineInstance class).

Simplify transition bootstrap logic by inlining bootstrapEnter.

Removed explicit var typing where implicit is sufficient.

Removed Behaviour type and just used Array of Action in place.

Remove Selector type and functions; replace with virtual methods on Vertex subtypes.

Implement a visitor pattern for state machine models.

Migtated transition bootstrap to a visitor.

Minor changes to StateMachine to enable it to be embedded within other state machines.

Remove evaluateCompletions method and just inline the code where used given its simplicity.

Remove assert function as it was used only once.

## Version 5.0.1
Fix bug relating to external transitions and orthogonal regions that could result in an invalid current active state.

Remove protected keywords due to a lack of tool support for it. 

## Version 5.0.0
Version 5 is a complete re-write from version 4.x.x:
- Much better performance by pre-computing all steps required during a state change. A clean/diry state is maintained  and re-computing possible if the machine strucutre changes.
- The API has changed to a fluent style enabling transitions to be defined in a more natural way.
- The code is authored in TypeScript; this hopefully will lead to better quality code. State machines using state.js can be authored in JavaScript of TypeScript.
