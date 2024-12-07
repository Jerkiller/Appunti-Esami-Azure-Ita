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

---

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
  * yb subj amatte exp basic data analysis, parttime
  * wb subj amatte exp parttime
* milestones
  * high-level documented
  * realistic but aggressive
  * planned start and end dates for each phase
* everyone must agree on scope benefit target and timeline
* why projects fail?
  * failure to define proj correctly
  * scope creep
  * communication


##### step 3

> SIPOC / COPIS
* high level process map (end to end)
* one of the 7 quality tools
* use it when -> need understanding in 1 page
* it describe the "as-is" not the "as-should-be"

steps:

* identify 5-7 H-L steps of proc
* identify customer who receive the outputs and the outputs
* identify all inputs (without them, the function doesn't work), and who are their suplliers
* verify the flow and validate with stakeholders

---

#### MEASURE

##### Step 4 identify what to measure

> process map
* describes steps of process in time, flow of info across depts
* identifies roles and responsabiltiies
* identifies dependendcies and bottlenecks
* interaction betw proc and tech requirements
* How to do it?
  * do it with a team (people who do it), validate, ask a lot, find issues
  * stay high level
  * map it "as-is"
  * map it across he organization
  * start from the borders: define start and end first
* swimlane diagram (lanes like a swimmingpool)
* 

common symbols
* oval - terminal activity
* rect - activity
* diamon - decision/question - M-options or binary
* arrow - connector - flow and direction of proc
* D - delay - when the process tempor. halts and what has to happen to resume
* rect paper - document - written docum is prepared name of doc
* DB
* triangle-down - storage - what's stored and for how long
* window rect - predefined process

**WASTE / NVA**
> anything other than the minimum amount of info, equipment, material, effort required to add value to product or services (definition of NVA non-value-added)
* the customer is unwilling to pay it
* it should be eliminated
* where is waste? 8 major wastes help us identify them

Every step of process can be:
* value add
* NVA
* business value add

**8 wastes**

1. DEFECTS/REWORK
   * rejects that req additional time, rss, $ to fix
   * rejects/scrap in production
   * not in good order transaction in services
   * missing info or rework loops or breaks
2. OVERPRODUCTION
   * producing too much or too soon
   * more info requested than required
   * effort not aligned wth risk complexity or cst needs
   * poorly applied automation
3. WAITING
   * a work has to stop for some reason
   * next in line is overwhelmed
   * something broke down
   * waiting for approval
   * run out o something
4. NONUTilized skills
   * fail to utilize time & talents of ppl
   * experts doing simple tasks
   * missing training or team work
   * poorly defined jobs & expectations
5. TRANSFER / TRANSPORTATION
   * excessive back and forth
   * movements across locations buildings
6. INVENTORY
   * work stuck in mail inboxes, fixed assets
   * bottlenecks in styaging WIP
   * idle or underutilized eqipment
7. MOTION
   * inefficient placement of rss creating motion
   * office rss disposition
   * distance betw workstations
   * poor housekeeping
8. EXCESS PROCESSING
   *  similar info in several places
   *  large variation in tim to do similar tasks
   *  unnecessary approvals

> DOWNTIME - acronym of 8 wastes

##### Step 05

* there is underlying cause of every defect. Defect = Y cause = x.
* for every Y, possibly many xs. Y = f(x)
* e.g. Y = high cycle time, x = system latency, missing training employees, ..
* SS -> focus on causes xs. Work on them and Y will reduce automatically
* Data classification = discrete(attribute data, based on counts, integers only) vs continuous data (variable data, like time or length or percentages, anything that can be bifurcated into meaningful fractions)
* data collection plan - how, who, when, what data to collect
   * HOW - data collection can be tedious to be done (>15 min per day is bad), try to do it automatically, standardize it with checksheet (1 of 7 basic tools)
   * WHO - Ppl can face dynamics like being afraid of data being known or be controlled or judged, ppl must know the benefits of measuring. PPl must be trained and exercise
  * WHEN - at 5pm? every hour?
* **SAMPLING** - when it's impractical, costly in money or time - collect only a portion of meaningful data
   * simple random sampling - same probability to pick up a type or another one, like taking MnMs colour from bag
   * stratified random sampling - divide population by character and then sample N for each group. Like divide MnMs by colour and take 3 for each
   * systematic sampling - take every nth element, like odds, evens or 1 every 10...
* minitab - 1972 called omnitab penssylvania university
   * 1500$ single license (or free trial)
   * SPSS IBM more priced and more complicated
   * Sigma XL not complex cheaper than minitab
* minitab - 2 main windows worksheet and menu with all functionalities
* pvalue - output of all statistical analysis performed in minitab
   * reference value is 0.05. >0.05 - data is stable <0.05 data unstable

##### Step 06

Identify data characteristics
* measures of central tendency
   * **mean**. Disadvantage: it's meaningless with extreme values (skewed data)
   * **median**
   * **mode** - most frquently data point. only with repeated values
* measures of dispersion/variation (std dev)
   * avg distance of all data poits from the mean
   * mean = 65%. stddev = +/- 3% => data from 62-68%
* minitab - Stat - Basic Statistics - select which stats, which variable

Check for data stability

* each business process have variation - data reflects it and output too
* stable data has small fluctuations
* use runcharts (1 of 7 basic tools)
* Minitab - stat - quality tools - Run chart - select column and subgroup = 1
   * if a single p-value of clusters mixtures trends or oscillations is < 0.05 data is unstable
   * if data is unstable, investigate the reason. you could collect more data. Don't use unstable data for analysis!
   * if data is stable go to step 7

Identify defects, defectives, unit and opportunities for error (OFE)

* OFE = # of elements that can cause a possible defect
* defects = # of errors/problems. a defect is a failure to meet customer req or performance standard
* unit = element being processed (or sampled) that can have multiple defects but can be defective or non difective (1 defect could be acceptable)
* defectives = # of bad units

Process Capability

* DPMO method to calculate sigma
  * compute DPO = D / (OfE * U) # defects OfE units
  * compute DPMO = D * 1M # defects per million opportun.
  * lookup DPMO to nearest sigma table conversion


#### ANALYZE

> Why a probl occurs?
> Perform and validate analysis, then review with a proc champion and go to improve

##### Step 07

**Identify performance gaps**

Process map review

* place meeting to review together map with clear agenda
* document problms, disconnections, opportunities, measures
* annotate thoughts
* validate things together, not in solution mode, but can start thinking of it

Brainstorming

* Used when creativity must be boosted
* both for individual or group usage (4-8)
* tips
  * 4-8 ppl
  * rules well defined
  * ppl already did something at home
  * short breaks (5 every 15-30)
  * warmup if group is new
  * start/end with an "around the room"
  * ideas visible and recorded
  * keep topic open after the brainst.
  * number items
  * ppl speak 1 at a time
  * awarness of criticism or phrase
  * checklist of techniques to expand ideas: modify, magnify, minify, subst.
  * establish need for brainst
  * enforce everyone's participation
  * stop early deviations from main topic
  * never judge ideas, crazy are welcome
  * appreciate and thx everyone
* pros
  * consensus, buy-in
  * everything by everyone on the table
  * promote unique ideas and validation
* cons
  * can b slow
  * only 1 can be done with the team, with many ppl

FISHBONE

* AKA Ishikawa
* useful to relate together and sum up brainst ideas
* one of the 7 quality tools
* find root causes of a probl
* identify causes from syntoms
* option: use 5M cathegories Man Method Machine Material MotherNature
* steps:
  * brainstorm possible causes
  * decide categories
  * identify most important causes
  * ask 5 why for ideas
  * brainstorm for cat that contains fewer items

5Ys
* one of the 7 quality tools
* useful because simple, no statistics required, good probing technique
* everyone agrees on RC? keep asking. RC can be the 4th or 9th Y.
* when RC, develop corrective action and responsible for it


##### Step 08

ASCERTAIN TYPICAL RC

* control impact matrix
* useful to prioritize RCs
* assemble focus group with subject matter expert
* list all possible RC from step 7
* divide RC out of control from in control
* classify RC based on impact high med low
* create matrix 3x2. Then focus only on high (medium) in control causes.

##### Step 09

> Validate RC

* line - if X is time, useful to infer derive
* bar - 1 or 2 categories clearly above / below the others the mean (e.g. Average Handle Time for teams D and E)
* pie chart - 2-6 percentage proportional categfories
* pareto chart - 20% causes contribute to 80% of the problem
* histogram chart - showcase data in bins (one of 7 basic quality tools) - check spread, skewness of data
* scatter chart - useful to relate 2 contin variable -> doesn't indicate causation
  * see points cloud? positive correlation if y=+x negative if y=-x
* correlation analysis
  * used in conjunction with scatter plot
  * objective data!
  * r = pearson correlation cohefficient
  * r in [-1;1]
  * 1 => positive corr / -1 => negative / 0 => no correlation
















