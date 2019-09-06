## An Elegant Puzzle: Systems of Engineering Management

by Will Larson

### Chapter 2: Organizations

#### 2.1: Sizing teams

* pg 32: Managers should support six to eight engineers
  * pg 32: Managers supporting fewer than four engineers usually act as TLMs, taking on design and implementation work.
  * pg 32: Managers supporting more than eight engineers usually act as coaches and safety nets for problems.
* pg 32: Managers-of-managers should support four to six managers
* pg 33: On-call rotations want eight-engineers
  * pg 33: This is for a two-tier 24/7 rotation.
  * pg 33: Shared rotations are not long-term solutions because it is stressful for people to be on-call for components they're unfamiliar with.
* pg 33: Small teams (fewer than four members) are not teams
  * pg 34: To reason about their deliery you must know about each on-call shift, vacation, and interruption.

#### 2.2: Staying on the path to high-performing teams

##### 2.2.1: Four states of a team

* pg 35: _A team is falling behind_ if each week their backlog is longer than it was the week before.
* pg 35: _A team is treading water_ if the team can get critical work done, but cannot start paying down technical debt or start major new projects.
* pg 36: _A team is repaying debt_ if they can start to pay down technical debt and are beginning to see the benefit of that repayment snowball.
* pg 36: _A team is innovating_ when technical debit is sustainably low, morale is high, and the majority of work is satisfying new user needs.

##### 2.2.2: System fixes and tactical support

* pg 37: _When the team is falling behind_, hire more people to move to treading water.
  * pg 37: Avoid re-assigning other people to the team, as people are not fungible and generally folks end up in useful places.
* pg 37: _When the team is treading water_, consolidate efforts to finish more things, and reduce concurrent work until you move to repaying debt.
* pg 37: _When the team is repaying debt_, add more time, thereby allowing the value of paying down that debt to compound.
* pg 37: _When the team is innovating_, maintain slack to sustain quality, but ensure your team isn't doing science projects that will lead to it being defunded.

##### 2.2.3: Consolidate your efforts

* pg 39: If most teams are falling behind, then hire onto one team until it's staffed enough to tread water, and then move onto the next.
* pg 39: Adding new members to a team requires the team to re-gel, so rapidly grow a team and then stop growing it to minimize the disruption.

#### 2.3: A case against top-down global optimization

##### 2.3.1: Team first

* pg 40: Sustained productivity comes from high-performing teams, and disassembling such a team leads to significant loss of productivity, even if its members are retained.
* pg 40: Moving individuals across teams can reset the clock on re-gelling, and so preserving a team's gelled state requires restrained growth.

##### 2.3.3: Slack

* pg 41: Teams put spare capacity to great use by improving areas within their aegis, in both incremental and novel ways.

##### 2.3.4: Shift scope, rotate

* pg 42: Instead move scope between teams, preserving the teams themselves. This works better because it avoids re-gelling costs, and preserves system behavior.
* pg 42: Undoing this also entails less disruption than undoing a staffing change would.

#### 2.4: Productivity in the age of hypergrowth

##### 2.4.1: More engineers, more problems

* pg 44: If you're doubling every six months and it takes six to twelve months to ramp up, then untrained engineers outnumber trained ones, and each trained engineer is devoting much of their time to training efforts.

##### 2.4.2: Systems survive one magnitude of growth

* pg 47: Most system-implemented systems are designed to support one to two orders-of-magnitude growth from the current load.
* pg 47: If your traffic doubles every six months, then your load increases by an order of magnitude every 18 months.
* pg 47: Under these conditions you'll have to re-implement your system twice every three years.
* pg 47: The real productivity killer is not system rewrites but the migrations that follow, which expand its consequences to the entire surrounding organization.

##### 2.4.3: Ways to manage entropy

* pg 48: You only get value from projects when they finish: to make progress, above all else, you must ensure that some of your projects finish.
* pg 49: To minimize the impact of interruptions, funnel them into an increasingly small area and then automate that area as much as possible: require tickets, create chatbots to create tickets, create runbooks, etc.
* pg 50: The best solution to frequent interruptions is a culture of documentation, documentation reading, and functional documentation search.
* pg 51: Avoid the gatekeeper pattern, which create odd social dynamics and are rarely a great use of a person's time.

##### 2.4.4: Closing thoughts

* pg 51: When faced with urgent project requests when underwater, learn to say no in a way that is appropriate to your company's culture.

#### 2.5: Where to stash your organizational risk?

* pg 51: Organizational debt is a sibling of technical debt, and includes things like biased interview processes and inequitable compensation.
* pg 52: The volatile subset of organizational debt that is likely to come abruptly due is called _organizational risk_.
* pg 52: With so much organizational risk, identify a few areas to improve, ensure you're making progress on those, and give yourself permission to do the rest poorly.
* pg 52: Stabilize team-by-team and organization-by-organization. Only delegate solvable risk; if something isn't likely to go well, you should hold the bag.

#### 2.6: Succession planning

##### 2.6.1: What do you do?

* pg 53: First, figure out what you do: examine your role in meetings, your calendar, your recurring processes, how you support individuals, your emails, and your to-do lists.

##### 2.6.2: Close the gaps

* pg 55: From your list, identify individuals who can immediately take over work. For items without someone who is ready today, identify individuals who could potentially take over.
* pg 55: For the remaining items, separate into easy and risky gaps, and write up a plan to close all of the easy gaps and one or two of the risky gaps.

### Chapter 3: Tools

#### 3.1: Introduction to systems thinking

##### 3.1.1: Stocks and flows

* pg 62: An accumulation of changes, such as the number of trained managers in your company, is called a stock. They are the memory of changes over time.
* pg 62: A change to a stock is called a flow, and each flow is either an inflow or an outflow.

#### 3.2: Product management: exploration, selection, validation

* pg 65: Product management is an iterative elimination tournament, with each round consisting of _problem discovery_, _problem selection_, and _solution validation_.

##### 3.2.1: Problem discovery

* pg 66: Taking the time to evaluate which problem to solve is one of the best predictors of a team's long-term performance.
* pg 66: Themes of analysis include _users pain_, _users purpose_, _benchmarking_, _competitive advantages_, _competitive moats_, and _compounding leverage_.
  * pg 66: Moats represent a sustaining competitive advantage, which makes it possible for you to pursue offerings that others simply cannot.
  * pg 67: Identify tasks that don't seem important enough to prioritize, but whose compounding value makes the work possible to prioritize.

##### 3.2.2: Problem selection

* pg 67: To curate a problem portfolio, consider _what's needed to survive this round, the next round, and to win a round_, _different time frames_, _industry trends_, and _return on investment_.
  * pg 68: Disagreement about what to work on is often rooted in different assumptions about what time frame to optimize for.
  * pg 68: Usually people under-prioritize quick, easy wins.

##### 3.2.3: Solution validation

* pg 68: Effective techniques include _writing a customer letter_, _identifying prior art_, _finding first users_, _experimenting_, _building cheaply_, and _ensuring low switching costs_.
  * pg 69: Prefer experimentation over analysis, as it's far easier to get good at cheaply validating than it is to get great at always picking the right solution.

#### 3.3: Visions and strategies

##### 3.3.1: Strategies and visions:

* pg 70: Strategies are grounded documents that explain trade-offs and actions to address a specific challenge.
* pg 70: Visions are aspirational documents that enable individuals who don't work closely together to make decisions that fit together cleanly.

##### 3.3.2: Strategy

* pg 71: An effective structure for good strategy has three sections: _diagnosis_, _policies_, and _actions_.
* pg 71: The diagnosis describes the challenge, calling out the factors and constraints that define it. It is a very thorough problem statement.
* pg 72: Policies describe the general approach you'll take to address the challenge. Bad guiding policies entrench the status quo, and good ones take a clear stance on competing goals.
* pg 72: Actions are the steps to implement policies. Good actions make you feel both uncomfortable and hopeful, while bad actions shirk changing anything.

##### 3.3.3: Vision

* pg 73: An effective vision helps people think beyond the constraints of their local maxima, and lightly aligns progress without tight centralized coordination.
* pg 73: A good vision defines a _vision statement_, _value proposition_, _capabilities_, _solved constraints_, _future constraints_, _reference materials_, and a _narrative_.
  * pg 73: The _vision_statement_ is a one- or two-sentence aspirational statement to summarize the rest of the document.
  * pg 73: The _value proposition_ defines how you will be valuable to your users and to your company, and what kinds of success you will enable them to achieve.
  * pg 74: The _solved constraints_ explain what you're limited by today but will no longer be constrained by in the future.
  * pg 74: The _narrative_ is a one-page summary of everything that is easy to digest. This typically immediately follows the vision statement.
* pg 74: A vision is succeeding when people reference it to make their own decisions, and is struggling when decisions keep happening that don't fit its direction.
* pg 75: Have one vision for every distinct area. Having two articulated visions in one place is worse than having zero.

##### 3.4: Metrics and baselines

* pg 76: Defining goals – good goals are a composition of four specific kinds of numbers
  * pg 76: A _target_ states where you want to reach.
  * pg 76: A _baseline_ indicates where you are today.
  * pg 76: A _trend_ defines the current velocity.
  * pg 76: A _time frame_ sets bounds for the change.
* pg 76: Investments and baselines
  * pg 76: Investments describe a future state that you want to reach, and baselines describe aspects of the present that you want to preserve.
  * pg 77: Baseline metrics are useful for narrowing the solution space that you explore in order to accomplish your investment goals.
* pg 77: Plans and contracts
  * Specify as few investment goals as possible, maybe three, and focus discussion on those. You should identify more baseline goals.
