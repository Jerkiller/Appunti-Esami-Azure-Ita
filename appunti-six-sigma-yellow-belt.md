# LSS YB

## Resources

* [Study Guide](https://aalssc.org/wp-content/uploads/2023/08/AALSSC-study-guide-Yellow-2023.pdf)

## Notes

* Six Sigma from deviation of a process
  * 3.4 defects per million opportunities (DPMO)
  * Process must be repetitive in its output, input, times... around the mean
* Waste is a silent saboteur
* DMAIC Define Measure Anlyze Improve Control
* TIM WOODS
  * Transport
  * Inventory (and WIP)
  * Movement
  * Waiting
  * Over production
  * Over processing
  * Defects
  * Skills
* VoC - what the customer wants and needs (must know who are the customer and what they value)
* CtQ - set of goals that are **Critical to Quality** that must be satisified
  * CtQ-Flowdown/Tree - tree diagram from VoC to CtQ
  * QDF - Quality Function Deployment - house of quality map CtQ and VoC
  * CtQ like must be quick in answering calls, specified CtQ like must answer in less than 30 sec
  * Prioritize CtQ
* See the process / start to finish or finish to start. Measure, speak, ask quetions. Gemba walk, process walk
  * To optimize movement, spaghetti diagram (map with curves of how people moves during process)
* SIPOC or COPIS - scheme to map the process / diagram Supplier Input Process Output Customer
  * to avoid "scope creep": uncontrolled expansion of requirements and growth in scope
* Value Stream Map - 3 kinds: Current Ideal and Future states
  * Swim lanes - VSM where material moves between departments
  * what to include? every subprocess has boxes with data, include times, waiting times, resources, quality metrics, arrows, inventory supplies
  * add stars for kanban bursts (initiatives)
  * Lead Time - time to go through the process
  * Identify Value added steps and non value added ones.
    * Value added step if and only if: customer pays (only the first time and only if done correctly) and it changes shape, form, fit
    * In the non value steps there can be business value (something internally important or required) e.g. paying taxes
  * Cycle Time C/T - time for a pieces to be made
  * ChangeOver Time C/O - time to change production
  * Other KPIs [here](https://www.dmaic.com/value-stream-mapping-value-stream-analysis/)
* Who is going to do what? in which priority/order? -> define an **action plan**
  * PICK chart (Possible Implement Consider Kill)
  * 2x2 Quadrant - x=Effort^(-1) y=Benefit
    |P|I|
    +-+-+
    |K|C|

## AIGPE course

* process hygiene factor is the basic requirement for a 6s project. A disciplinary issue is an example (people doing what they want). Small lean projects, kaizen lean instead, basic business management practices.
* business ,ust be started more than 6 months earlier -> stable process
* employees must be disciplined and well versed

### AIGPE Exam
* 50 mult choice questions
* open book and no time limit
* 2 attempts then costs 50$
* translate projects into real scenario
* study all topics in detail
* complete activities several times
* complete practice questions
* simulation exam

### Kaizen vs Lean vs 6s

* Kaizen = change for the better (any **small improvement** in BP)
  * problems where the cause is known and solution is simple
  * 0-5 days to complete. 1 / team
* Lean = **Elimination of waste**
  * problems where the cause is known and solution is complex
  * tools like VSM poka-joke (fool-proof processes), heijunka (load leveling), jidoka (automation with human intervention), JIT (necessary material when needed)..
  * always team proj 45d-90d
  * not data-driven
* 6s - data driven methodology focused on standardize and **reduce variation**
  * a 6s project has 3.4 defects in one million opportunities
  * cause unknown and complex solution
  * 90d-180d team proj

### Sigma

* std dev = how spread from the mean are data
* process capability = capability of process to produce defect free work
* more sigma = less DPMO (defects per million opportunities) 2s = 300K defects :-D

### lean 6s

* combination of lean amd 6s techniques
* average industries perform at 3s level, while best-in-class at 6s
* human related industries perform at 2s level (like airlaines baggage service, doctors, lawyers...)

### Metodologies

- DMADV define measure analyze design verify
   - fot new product, process, serivice / very used in manufact
   - lesser freqient use
   - focus on creating right product with constr
- DMAIC idem                   improve control
   - require historical data
   - for ongoing businessm more frequent usage
   - goal of maximizing profits (sales productivity costs)
- 
### Six Sigma DMAIC
* 5 phases define analyze measure improve control
   * define: what problem trying to solve?
   * what is the entity?
   * why is it happeninhg?
   * what do tou propose to do?
   * how to control proposal execution?
* at end of each phase, toll gate review / meeting
* 15 steps with tools
* based on PDCA (plan = define, analyze = do, improve = check, control = act)
* proj = 3-6 months
* how long each phase? it depends... every proj is unique
* the only documented step vby step meth in the markey to solve every business problem

### YB role

* part time role
* help green belt
* execute smaller proj
* execute sipoc, proc map, brainstorm, rca, solution ident
* smetimes subject matter expert
* data collection actm basic data anal
* implement and monitor pilot
* update control charts and exec control plans
* mentored by a green belt

* soft skill will be developed
   * good facilitation
   * change management
   * analyt skills
   * leadership and communication

-

#### DEFINE

> addresses the problem to solve

* 3 steps:
   *  understand customer and business requirement
      *  VoC
   *  complete proj charter
      * proj chart how do you know there is a probl, which measures? what success look like? scope? team? proj plan? 
   *  complete high level as is process map
      * sipoc (40000ft above) where is the probl occurring
    
##### step 1
* customer is king, everything starts with customers
* capture stated unstated and anticipated customer requirements
* methods:
  * surverys (more generic than specific, email, fast feedback from many people. with time and money constraints it's useful)
  * interviews /smaller segment of customer, specific feedback. phone or in person with trained facilitator
  * focus groups (similar to interview but with group of cust at one time, meeting, if willing to participate with multiple customers, also via social
  * suggestion complaints, compliments or observations
* get specific customer verbatim comments /(feedbacks)

* need = desire/expectation from product or service
* requirement = attribute that fulfill a need (the must of a product), should be measurable
* how to translate voices to requirements? ask, involve customer, deep ask, translate terms to yours, review, prioritize. Use this template:
* VoC (verbatim, exact) - Critical customer criteria need - CTQ requirement (KPI, metric)
* I hope to book my flight without error through the call center - need: ACCURACY (issue correct flight tickets as communicated) - CtQ 100% accuracy issuing tickets

##### step 2

* project charter: document with a framework for improving project
* created at the beginning of dmaic proj
* it's a living document (uodated on a regularly basis)
* it includes
* problem statement
   * pain of customer
   * full of numbers, what is probl, when where, magnitude, consequences of the problem
   * be short, not wordy
   * no blame, no cause, no solution
* goal statement
  * define the improvement objective (starts with a verb improve eliminate control)
  * time bound
  * SMART specific measurable attainable/achieavable relevant timebound
  * no cause, blame or solutions
* project scope
  * start, end points
  * proj constraints and dimension
  * horizontal scope - end to end process
  * vertical scope - going in depth of team or location
  * define out of scope
* project team
  * selected by black and green belt
  * champion
  * mbb expert stats, drive vision, coach, review gates
  * bb drive large proj, leads tollgates, expert stats, coach train gb
  * gb drive process proj, participate tollgates, coach train yb and wb parttime
  * yb
* milestones
* everyone must agree on scope benefit target and timeline













