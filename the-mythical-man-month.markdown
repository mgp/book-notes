## The Mythical Man-Month

by Frederick P. Brooks, Jr.

### Chapter 1: The Tar Pit

#### The Joys of the Craft

* pg 7: Few media of creation are so flexible, so easy to polish and rework, so readily capable of realizing grand conceptual structures, as programming.
* pg 8: Programming is fun because it gratifies creative longings built deep within us and delights sensibilities we have in common with all people

#### The Woes of the Craft

* pg 8: Adjusting to the requirement for perfection is, perhaps, the most difficult part of learning to program.
* pg 9: The challenge and the mission are to find real solutions to real problems on actual schedules with available resources.

### Chapter 2: The Mythical Man-Month

* pg 14: Our techniques of estimating time for software development reflect an unvoiced assumption that all will go well, which is quite untrue.

#### Optimism

* pg 15: Dorothy Sayers in *The Mind of the Maker* divides the creative activity into three stages: the idea, the implementation, and the interaction.
* pg 15: The programmer builds from pure thought-stuff: concepts are very flexible representations thereof. Because the medium is tractable, we expect few difficulties in implementation; hence our pervasive optimism.

#### The Man-Month

* pg 16: Cost does indeed vary as a product of the number of programmers and the number of months. Progress does not.
* pg 16: Programmers and months are interchangeable commodities only when a task can be partitioned among many workers _with no communication among them_.
* pg 18: The added burden of communication is made up of two parts: training and intercommunication.
* pg 18: If a task must be separately coordinated with each other part, the effort increases as n(n-1)/2.

#### Systems Test

* pg 19: Because of optimism, we expect the number of bugs to be smaller than it turns out to be.
* pg 20: Failure to allow enough time for system test is disastrous: Since the delay comes at the end of the schedule, no one is aware of schedule trouble until almost the delivery date.
* pg 20: Delays at the end has severe financial repercussions, as the project is fully staffed, and cost-per-day is maximum.

#### Gutless Estimating

* pg 21: It's very difficult to make a vigorous, plausible, and job-risking defense of an estimate that is derived by no quantitative method, supported by little data, and certified chiefly by the hunches of managers.

#### Regenerative Schedule Disaster

* pg 24: Take no small slips: Allow enough time in a new schedule to ensure that the work can be carefully and thoroughly done, and that rescheduling will not have to be done again.
* pg 24: When secondary costs of delay are high, the manager's only alternatives are to trim the task formally and carefully, to reschedule, or to watch the task get silently trimmed by hasty design and incomplete testing.
* pg 25: Brooks's Law states that adding programmers to a late software project makes it later.
* pg 25: The number of months of a project depends upon its sequential constraints. The number of programmers depends on the number of independent subtasks.

### Chapter 3: The Surgical Team

#### The Problem

* pg 30: A study found that the ratios between the best and worst programmers averaged 10:1 on productivity measurements and 5:1 on program speed and space measurements.
* pg 31: For efficiency and conceptual integrity, one prefers a few good minds doing design and construction. The challenge is how to bring considerable manpower to bear on large systems.

#### Mills's Proposal

* pg 34: The tester is both an adversary who devises system test cases from functional specs, and an assistant who devises test data for the day-by-day debugging.

#### How It Works

* pg 35: In the "surgical team," there are no differences of interest, and differences of judgment are settled by the surgeon unilaterally.

#### Scaling Up

* pg 36: Reducing the number of minds determining the design improves the conceptual integrity of each piece and reduces the coordination cost.

### Chapter 4: Aristocracy, Democracy, and System Design

#### Conceptual Integrity

* pg 42: Most programming systems reflect conceptual disunity not from a serial succession of master designers, but from the separation of design into many tasks done by many programmers.
* pg 42: Brooks contends that conceptual integrity is _the_ most important consideration in system design.

#### Achieving Conceptual Integrity

* pg 43: Because ease of use is the purpose of a system, the ratio of function to conceptual complexity is the ultimate test of system design.
* pg 44: For a given level of function, the best system is the one which specifies things with the most simplicity and straightforwardness.

#### Aristocracy and Democracy

* pg 44: Conceptual integrity dictates that the design must proceed from one mind, or from a very small number of agreeing resonant minds.
* pg 44: The separation of architectural effort from implementation is a very powerful way of getting conceptual integrity on very large projects.
* pg 45: As Blaauw has said, where architecture tells _what_ happens, implementation tells _how_ it is made to happen.
* pg 46: Good features and ideas that do not integrate with a system's basic concepts are best left out.
* pg 46: If a system is to have conceptual integrity, someone must control the concepts. That is an aristocracy that needs no apology.
* pg 46: Implementers have different creative work, as the cost-performance ratio of the product will depend on him or her, while ease-of-use depends on the architect.

#### What Does the Implementer Do While Waiting?

* pg 49: Given some rough approximations as to the function of the system that will ultimately be embodied in the external specifications, the implementer can proceed.
* pg 50: A widespread horizontal division of labor has been sharply reduced by a vertical division of labor, and the result is radically simplified communications and improved conceptual integrity.

### Chapter 5: The Second-System Effect

* pg 54: The discipline that bounds the architect's inventive enthusiasm is thoroughgoing, careful, and sympathetic communication between architect and builder.

#### Interactive Discipline for the Architect

* pg 54: The architect has two possible answers when confronted with an estimate that is too high: cut the design, or challenge the estimate by suggesting cheaper implementations.

#### Self-Discipline – The Second-System Effect

* pg 55: The second system is the most dangerous system that an architect ever designs.
* pg 55: The general tendency is to over-design the second system, using all the ideas and frills that were cautiously sidetracked on the first one.
* pg 56: The second system also has a tendency to refine techniques whose very existence has been made obsolete by changes in basic system assumptions.
* pg 58: The architect must be conscious of the peculiar hazards of a system, and exert extra self-discipline to avoid functional ornamentation and to avoid extrapolation of functions that are obviated by changes in assumptions and purposes.

### Chapter 6: Passing the Word

#### Written Specifications – the Manual

* pg 62: The architect must always be prepared to show _an_ implementation for any feature he decides, but he must not attempt to dictate _the_ implementation.
* pg 62: The style of a manual must be precise, full, and accurately detailed. And so they make for dull reading, but precision is more important than liveliness.
* pg 62: The casting of decisions into prose specifications must be done by only one or two, if the consistency of the prose and product is to be maintained.

#### Formal Definitions

* pg 63: With English prose one can show structural principles, delineate structure, give examples, mark exceptions, and emphasize contrasts. Most important, one can explain _why_.
* pg 63: Formal definitions inspire wonder at their elegance and confidence in their precision, but demand prose explanations to make their content easy to learn and teach.
* pg 64: If one has both, one must be the standard, and the other must be a derivative description – and clearly labeled as such.
* pg 64: A formal definition can serve as an implementation, and an implementation can serve as a formal definition.
* pg 65: An implementation may over-prescribe the externals, it not only says what a machine must do, but it may also say a great deal about how it had to do it.
* pg 65: One must also refrain from modifications to the implementation while it is serving as a standard.

#### Conferences and Courts

* pg 66: One useful level of meeting is a conference of all the architects with the chief architect presiding:
  * pg 66: This group attempts to invent many solutions to problems, then a few solutions are passed to one or more architects for detailing into proposals.
  * pg 66: Minutes are kept and decisions are formally, promptly, and widely disseminated.
* pg 67: A "supreme court session" handles a backlog of minor appeals, open issues, and disgruntelements:
  * pg 68: In these meetings, everyone is heard, everyone participates, everyone better understands the intricate constraints and interrelationships among decisions.

#### The Telephone Log

* pg 69: A puzzled implementer must telephone the responsible architect and ask his or her question, rather than to guess and proceed.
* pg 69: A *telephone log* kept by the architect records every question and every answer. Each week these are aggregated among architects and distributed among implementers.

### Chapter 7: Why Did the Tower of Babel Fall?

#### Communication in the Large Programming Project

* pg 74: Schedule disaster, functional misfits, and system bugs all arise because the left hand doesn't know what the right hand is doing.
* pg 75: Regular project meetings, with one team after another giving technical briefings, are invaluable.
* pg 75: A *project workbook* imposes structure on documents that will be produced anyway; all documents of the project must be a part of its structure.
* pg 75: Technical prose is almost immortal.
* pg 76: A project workbook helps ensure that relevant information gets to all the people who need it.
* pg 76: Of critical importance is timely updating: The workbook must be current.

#### Organization in the Large Programming Project

* pg 79: The means by which communication is obviated are *division of labor* and *specialization of function*.
* pg 79: The inadequacies of a tree-like organizational structure give rise to staff groups, task forces, committees, and other organizational structures.
* pg 79: A *producer* assembles the team, divides the work, and establishes the schedule; the *architect* conceives the design, identifies its subparts, and sketches its internal structure.
* pg 80: Organizations must be designed around the people available; not people fitted into pure-theory organizations.
* pg 80: If the producer is the boss of the architect, the difficulty is to establish the architect's authority to make technical decisions without impacting his time.
* pg 83: The producer as boss is a more suitable arrangement than the "surgical team" for the larger subtrees of a really big project.

### Chapter 8: Calling the Shot

* pg 88: "Extrapolation of the hundred-yard dash shows that a man can run a mile in under three minutes."
* pg 88: A survey at System Development Corporation shows *effort* = *constant* x (*number of instructions*) ^ (1.5).
* pg 90: Many estimates make an unrealistic assumption about the number of technical work hours per programmer per year.

#### Corbató's Data

* pg 94: Productivity seems constant in terms of elementary statements.
* pg 94: Programming productivity may be increased as much as five times when a suitable high-level language is used.

### Chapter 9: Ten Pounds in a Five-Pound Sack

#### Program Space as Cost

* pg 98: Like any cost, size itself is not bad, but unnecessary size is.

#### Size Control

* pg 100: Define exactly what a module must do when you specify how big it must be.
* pg 100: Beyond policing, fostering a total-system, user-oriented attitude in the implementers may well be the most important function of the programming manager.

#### Space Techniques

* pg 101: For a given function, the more space, the faster. This is true over an amazingly large range.

#### Representation Is the Essence of Programming

* pg 102: "Show me your flowcharts and conceal your tables, and I shall continue to be mystified. Show me your tables, and I won't usually need your flowcharts; they'll be obvious."
* pg 103: Disentangling yourself from your code and contemplating your data is invaluable, for representation _is_ the essence of programming.

### Chapter 10: The Documentary Hypothesis

* pg 108: A document can serve as a major occasion for focusing thought and crystallizing discussions that otherwise would wander endlessly. Its maintenance becomes a surveillance and warning mechanism.

#### Documents for a Computer Product

* pg 108: Existence of the budget forces technical decisions that otherwise would be avoided; and, more important, it forces and clarifies policy decisions.

#### Documents of a Software Project

* pg 110: No matter how small the project, begin to immediately formalize at least mini-documents to serve as your database.
* pg 111: Conway's Law says "Organizations which design systems are constrained to produce systems which are copies of the communication structures of those organizations."
* pg 111: If the system design is to be free to change, the organization must be prepared to change.

#### Why Have Formal Documents?

* pg 111: First, writing decisions down is essential, as only then do the gaps appear and the inconsistencies protrude.
* pg 111: Second, the documents will communicate the decision to others.
* pg 111: Finally, a manager's documents gives him a data base and checklist, where he can see progress and where changes of emphasis or shifts in direction are needed.
* pg 112: The task of the manager is to develop a plan and then to realize it. But only the written plan is precise and communicable.

### Chapter 11: Plan to Throw One Away

#### Pilot Plants and Scaling Up

* pg 116: In most projects, the first system built is barely usable. It may be too slow, too big, awkward to use – or all three.
* pg 116: You will always discard and redesign. Plan to throw one away; if you don't plan this, you may deliver what you want to throw away to customers.

#### The Only Constancy is Change Itself

* pg 117: The throw-one-away concept is itself just an acceptance of the fact that as one learns, one changes the design.

#### Plan the Organization for Change

* pg 118: Reluctance to document designs comes from the designer's reluctance to commit to the defense of decisions which he or she knows to be tentative.
* pg 118: Each programmer must be assigned to jobs that broaden him or her, so that the whole force is technically flexible.
* pg 118: On a large project the manager needs to keep two or three top programmers as technical cavalry that can gallop to the rescue wherever the battle is thickest.
* pg 119: A reassignment from the technical ladder to the managerial one must not come with a raise, and must be announced as a "reassignment" and not a "promotion."

#### Two Steps Forward and One Step Back

* pg 121: The cost of maintaining a program is strongly affected by the number of users, as more users find more bugs.
* pg 122: Defects are not fixed cleanly because:
  * pg 122: The defect shows itself as a local failure, while its system-side ramifications are non-obvious.
  * pg 122: The repairer is not the programmer who wrote the code, and may be a junior programmer.

#### One Step Forward and One Step Back

* pg 122: All repairs tend to destroy the structure and increase the entropy of a system. As time passes, the fixing of the system ceases to gain any ground.
* pg 123: Systems program building is an entropy-decreasing process.
* pg 123: Program maintenance is an entropy increasing process, and even its most skillful execution only delays the subsidence of the system into unfixable obsolescence.

### Chapter 12: Sharp Tools

#### Vehicle Machines and Data Services

* pg 132: Preproduction hardware does not work as defined, does not work reliably, and does not stay constant.
* pg 132: Consequently a dependable simulator on a well-aged vehicle retains its usefulness far longer than one would expect.

#### High-Level Language and Interactive Programming

* pg 134: Over-documentation is far preferable to the under-documentation that characterizes most programming systems.
* pg 135: The chief reasons for using a high-level language are productivity and debugging speed.
* pg 135: There are fewer bugs because one avoids an entire level of exposure to error, a level on which one makes not only syntactic errors but semantic ones.

### Chapter 13: The Whole and the Parts

#### Designing the Bugs Out

* pg 142: The most pernicious and subtle bugs are system bugs arising from mismatched assumptions made by the authors of various components.
* pg 142: Many failures concern exactly those aspects that were never quite specified.
* pg 143: Using top-down design, one can identify modules of solution or of data whose further refinement can proceed independently of other work.
* pg 144: Many poor systems come from an attempt to salvage the bad basic design and patch it with all kinds of cosmetic relief.

#### System Debugging

* pg 147: Common sense, if not common practice, dictates that one should begin system debugging only after all the pieces seem to work.
* pg 148: Build plenty of scaffolding, or programs and data built for debugging purposes but never intended to be part of the final product.
* pg 149: Add one component at a time when debugging, even though optimism and laziness tempt us to violate it.
* pg 150: One must assume that there will be lots of bugs, and plan an orderly procedure for snaking them out.

### Chapter 14: Hatching a Catastrophe

* pg 154: With schedule slippage, major calamities are easier to handle, as one responds with major force, radical reorganization, the invention of approaches. The whole team rises to the occasion.
* pg 154: Day-to-day slippage is harder to recognize, harder to prevent, and harder to make-up.

#### Milestones or Milestones?

* pg 154: To control a big project on a tight schedule, one must first have a schedule. Each of a list of events, called milestones, has a date.
* pg 154: Milestones must be concrete, specific, measurable events, defined with knife-edge sharpness.
* pg 155: It is more important that milestones be sharp-edged and unambiguous than that they be easily verifiable by the boss.
* pg 155: If a milestone is fuzzy, we often deceive ourselves. No one enjoys bearing bad news to the boss, and so we soften it without any real intent to deceive.
* pg 155: A fuzzy milestone deceives one about lost time until it is irremediable, and chronic slippage is a morale-killer.

#### "The other Piece Is Late, Anyway"

* pg 155: Hustle provides the cushion and reserve capacity for a team to cope with routine mishaps and to forfend minor calamities. A calculated response dampens hustle.
* pg 156: In determining which slips matter, there is no substitute for a PERT chart or a critical-path schedule.
* pg 156: The PERT chart shows how much hustle is needed to keep one's own part off the critical path, and it suggests ways to make up the lost time in the other part.

#### Under the Rug

* pg 157: Every boss needs two kinds of information: Exceptions to plan that require action, and a status picture for education.
* pg 157: So long as a first-line manager thinks he or she can solve something alone, that manager will not tell his or her boss.
* pg 157: There are two rug-lifting techniques open to the boss:
  * pg 157: If the boss disciplines himself to accept status reports without panic or preemption, the first-line manager will come to give honest appraisals.
  * pg 158: Or the manager can require a report showing milestones and actual completions.
* pg 160: A *Plans and Controls* team has no authority except to ask all the line managers for updates on their milestones, from which it creates a PERT chart.
* pg 160: The Plans and Controls team is the watchdog who renders the imperceptible delays visible and who points up the critical elements.
