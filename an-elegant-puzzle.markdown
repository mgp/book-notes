## An Elegant Puzzle: Systems of Engineering Management

by Will Larson

### Chapter 2: Organizations

#### 2.1: Sizing teams

* pg 32: Managers should support six to eight engineers
  * pg 32: Managers supporting fewer than four engineers usually act as TLMs, taking on design and implementation work.
  * pg 32: Managers supporting more than eight engineers usually act as coaches and safety nets for problems.
* pg 32: Managers-of-managers should support four to six managers
* pg 33: On-call rotations want eight-engineers
  * pg 33: This is for a two-tier 24/7 rotation
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
