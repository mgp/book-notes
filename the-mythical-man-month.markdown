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
