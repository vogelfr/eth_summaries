---
title: HCI Summary
desription: Summary of 252-3110-00L Human Computer Interaction at ETH Zürich HS16
tags: ETH, Zusammenfassung
---
HCI Summary
===
Frédéric Vogel `vogelfr@ethz.ch`, ETH Zürich, HS16
[TOC]

<p style="page-break-after:always;"></p>

# Introduction
## Definitions
Human Computer Interaction
~ Human-computer interaction is a discipline concerned with the **design, evaluation and implementation of interactive computing systems** for human use and with the study of major phenomena surrounding them

Human
~ Single user (end user of program)
~ Others
    - Local (friends, colleagues)
    - Remote (social networks, remote collaboration)

Computer
~ Machine running program
~ Often distributed
~ Many different form factors and capabilities

Interaction
~ User provides input
~ Machine communicates results

User Interfaces (UI)
~ Main point of contact between user and running program
~ Graphical UI
~ Hardware is an integral part of the UI

**_The HCI Process_**
<img src="https://i.imgur.com/7cxVoUa.png" style="float:right; width:350px;"></img>
![](https://i.imgur.com/fsoZE1r.png =350x)

<p style="page-break-after:always;"></p>

# The User
## Components of Cognition
- Perception
    - Visual system
    - Auditory System
    - Haptic System
- Memory
    - Sensory memory
    - Short-term memory
    - Long-term memory
- Action
    - Motor system
![](https://i.imgur.com/dZi8ZOf.png)

<p style="page-break-after:always;"></p>

## Model Human Processor
### Definition
Model Human Processor
: Basic model that gives us an *abstracted* understanding of
    - Perception
    - Memory
    - Motor System
: Can be applied to measure (predict)
    - Execution time
    - Error rates
    - Training effects for simile stimulation / reaction interactions

### Overview
- Three ++processors++ with associated ++memory++
- Perceptual System
    - Sensors and buffers
- Cognitive System
    - Working memory content symbolically coded
- Motor System
    - Movements
- Each processor has associated runtime
    - Overall system runtime sum of these

### Perception (Visual System)
- Visual field:
    - Horizontal: 60° nasally -- +90° temporally
    - Vertical: -70° -- +60°
- Foveal vision (fine details) within 2° radius of fovea
- Retina decodes light through photo receptors
    - Rods (low-light vision):
        - Very light sensitive
        - Slow response time
        - In periphery
        - Maximum sensitivity at 500nm
    - Cones (color vision):
        - Less light sensitive
        - Fast response time
        - Concentrated at fovea
        - 3 Types:
            - S type (420nm, blue)
            - M type (534nm, green)
            - L type (564nm, red)
- Useful Field of View: 1° -- 4°, max 15°

#### Object Based versus View Based Representation
Object centric representation
~ Description of objects as combination of structural parts in 3D, view independent

View centric representation
~ Description of objects as view dependent 2D projections with hierarchical recognitions along ventral pathway, from low-level features to complex objects
~ View based models more likely accurate
### Perceptual Processor
- Receives sensor signals and stores in buffers (one buffer/sensory channel)
- Perception time $\tau_P \approx 100 \texttt{ms}$ (range: 50 -- 200ms)
- Bloch's Law: $R=I\cdot t$
    - $R$: Response, $I$: Intensity, $t$: exposure time
    - $R$ is constant for $t<100\texttt{ms}$

### Cognitive Processor
- Operates on chunks of information
- Processing time $\tau_C \approx 70 \texttt{ms}$
- Divided into short (STM) and long (LTM) term memory

### Motor Processor
- Controls and runs motor system
- Without perceptual control: $\tau_M \approx 70\texttt{ms}$

Total Processing time (middle-man times): $\tau_P + \tau_C + \tau_M \approx 240\texttt{ms}$

<p style="page-break-after:always;"></p>

## User Modeling
### Fitts' Law
Models throughput in aimed movements
$MT = I_M \cdot I_D$
- Index of Difficulty $I_D = \log_2(\frac{2D}{W})$
- Index of Movement $I_M = 100\texttt{ms}/\texttt{bit}$

Fitts' Thesis
~ Fixed information-transmission capacity of the motor system

- $ID =$ # of bits required to specify movement
- Index of performance $IP = \frac{ID}{MT}(\frac{\texttt{bits}}{\texttt{ms}})$

#### Implications
- Doubling distance adds roughly a constant to execution time
- Doubling target width (within limits) is roughly equal to halving distance
- *Bigger benefit* to increasing size of *small targets* than large targets

#### MHP to Fitts' Law
Movement consists of multiple sub-movements of *constant* time $t$ and *constant* error $\varepsilon$

- Time to move to a target is $n \cdot (\tau_P + \tau_C + \tau_M)$
- 1st cycle: $X_1 = \varepsilon X_0 = \varepsilon D$
- 2nd cycle: $X_2 = \varepsilon X_1 = \varepsilon(\varepsilon D) = \varepsilon^2 D$
- $n^{th}$ cycle: $X_n = \varepsilon^n D$
- Movement stops when in target area: $\varepsilon^n D \leq \frac{1}{2}W$
- $n = -\log_2(\frac{2D}{W})/\log_2\varepsilon$
- $MT = I_M \log_2(\frac{2D}{W})$ where $I_M=-\frac{\tau_P + \tau_C+ \tau_M}{\log_2\varepsilon}$

<p style="page-break-after:always;"></p>

#### Shannon Analogy
<img src="https://i.imgur.com/WPbvWrR.png" style="float:right; width:300px"></img>
- $a, b$ are constants
    - depend on input device and user skill
    - empirically derived
- $\log_2(\frac{A}{W}+1)$: Index of difficulty
- 2D Motion? $ID = \log_2(\sqrt{(\frac{A}{W})^2+\omega (\frac{A}{H})^2}+1)$

<p style="page-break-after:always;"></p>

## Analyzing the User Experience
#### Evaluation Types
Formative
: - Early in the design process
  - Sanity checks that you're building the right thing

Summative
: - Is our solution working?
  - Has it improved from last iteration?
  - Does it work better than other solutions?

#### Usability metrics
Effectiveness
~ Being able to complete a task

Efficiency
~ Amount of effort required to complete a task

Satisfaction
~ Degree to which the user was happy completing the task

**Find cause and effect!**
#### Designing an empirical study
1. What is being compared? (Independent variables)
2. What are they being compared in? (Dependent variables, "metrics")
3. What (else) is being varied? (Extraneous variables)

Types of variables:
- Categorical: unordered discrete values
- Ordinal: ordered discrete values
- Cardinal/Interval: continuous values

Main question: **Does `independent variable` cause differences in `dependent variable`?**

Extraneous variables
~ Similar to independent variables but we want to eliminate/control an effect

The (one) Equation
~ $\texttt{Outcome}_i = (\texttt{Model}_i) + \texttt{Error}_i$
~ $\texttt{Outcome}_i = (b_0 + b_1 X_{1i}) +\texttt{Error}_i$

T Test
~ $t = \frac{\overline{B}-\overline{A}}{Var_B}$ with $Var = \frac{1}{N-1}\sum_{i=0}^N(x_i - \overline{x})^2$
~ Limitations:
    * Assumes error term has Normal distribution
    * Sample size should be the same
    * All data should be independent
    * Interval type variables


<p style="page-break-after:always;"></p>

# The User Centered Design Process
<img src="https://i.imgur.com/VkDd2OV.png" style="float:right; width:200px"></img>


### Interaction Style
#### Established Interaction Paradigms
- Commands
- Dialogue Systems
- Searching and Browsing
- Direct Manipulation
    - Visibility of Objects and Actions
    - Rapid, reversible, incremental actions
    - Replacement of complex command-language syntax with direct, visual manipulation of object of interest

#### Notable Emerging Interaction Paradigms
- Context Sensitive UIs
- Natural Language Interfaces
- AR/VR

#### Designing Interactive Technologies
**Must consider all dimensions from hardware to UI!**
1. Sensing human activity
    - Noisy and sparse data
        - Huge variety of sensors
        - New types emerging
    - Diverse human behavior
        - Highly complex & non-linear
        - Context and user specific
2. Computational Interface Design
    - Goal: Enable non-experts to create complex interactive technologies
    - Designer-in-the-loop tools:
        1. Sensing based devices
        2. Interactive systems without domain knowledge

<p style="page-break-after:always;"></p>

# Gestures and Kinect
Gesture
~ Motion of the body that *contains information*.

## Formally Defined Gestures
Examples:
- Hand signs of traffic police
- Hand signs while diving

## Semiotic Gestures
- Symbolic
    ~ Single meaning within a culture
- Pantomimic
    ~ Show use of some invisible tool or object
- Iconic
    ~ Convey information about size, shape or orientation
- Deictic
    ~ Referential specifying an object or directing attention
    
## Kinect v2
<img src="https://i.imgur.com/tOZ2xIC.png" style="float:right; width:200px"></img>

- Full-body 3D motion capture, face tracking and speech recognition
- Skeletal tracking of up to 6 persons
    - Set of 25 joints per person
- Built-in microphone array to record and locate voices
- Promises to deliver a "Natural user Interface" experience, your body is the controller
- Hand tracking:
    - Hand state tracked for two bodies closest to sensor
    - Different states for each hand:
        - Open
        - Closed
        - Lasso
        - Unknown
        - NotTracked


<p style="page-break-after:always;"></p>

# Interaction Design
## Aim
- *Support users* rather then replace them
- Create technologies and applications to *enrich the user experience*
- Requires *better understanding* of users' needs and desires
- Users should be unaware of technologies as they *focus on task at hand*

## Design Process
![](https://i.imgur.com/ss0oGaz.png =500x)

1. Establish requirements
2. Develop a conceptual model
3. Produce rough prototypes
4. Experiment with alternative designs

### Scenarios
- Informal narrative descriptions: 
    Helps visualise things from user perspective, explores the actual/potential and usual/unusual
- Developed into use cases
- Storyboards used for establishing requirements and sketching out ideas, as well as for communication

### Sketching
- *Not* about drawing, about *design*
- Tool to help *express*, *develop* and *communicate* design ideas
- Part of a process:
    - idea generation
    - design elaboration and choices
    - engineering

### Prototyping
- Turning sketches into prototypes (user interaction possible)
- User feedback early in development process
- promotes agile development and user participation in design

#### Paper prototyping
- Early detection of basic flaws
- Fast and cheap
- Encourages creativity

Steps:
1. Create user profile
2. Decide on set of tasks
3. Create prototype
4. Perform walkthroughs
5. Plan and carry out study

Good tasks:
- Based on goal that matters to user
- covers questions important to success
- Neither too broad nor too narrow
- Finite and predictable set of possible solutions
- Clear end point that user can recognize
- Elicits action not just opinion

Good at:
- raising issues about requirements and functionality
- detecting unclear concepts and terminology
- problems related to navigation, task flow and workflow
- raising content issues
- defining documentation and help requirements
- identifying issues related to screen layout

Bad at:
- detecting interaction issues
- input methods
- response times
- subtle screen changes
- scrolling and animation

### Running a study
Facilitator
~ guides the process

Computer
~ simulates system dialogue

Observers
~ document the study

Helpers
~ Users can ask for help
~ Helper gradually provide more detail to determine information that gets user unstuck

## Design principles
![](https://i.imgur.com/6d1idPj.png =500x)

- **Visibility**
- **Feedback**
- **Constraints**
- **Mapping**
- **Consistency**
- **Affordances**

Design systems that are *easy to learn* and exploit things we do *automatically* rather than imposing *cognitive load*.

We *perceive* what we *expect*.

<p style="page-break-after:always;"></p>

### Gestalt Laws
![](https://i.imgur.com/A6B3nTt.png =500x)

Emergence
~ Process of complex pattern formation from simpler rules

Multistability
~ Tendency of ambiguous perceptual experience to pop back and forth unstably between two or more alternative interpretations

Reification
~ *Constructive* or *generative* aspect of perception by which perceived object contains more explicit spatial information than sensory stimulus on which it is based

Invariance
~ Property of perception whereby simple geometrical objects are recognized independent of rotation, translation and scale

Closure
~ Objects, such as shapes, letters and pictures, perceived as being whole when they are not complete

Similarity
~ Objects perceptually form a group if they are similar to each other in terms of shape, colour, shading, etc.

Proximity
~ Objects perceptually form a group if they are close to each other

Continuity
~ Objects tend to be integrated into perceptual wholes if they are aligned within an object

Use *spacing* and *similarity* to give *structure* in a lightweight way.

<p style="page-break-after:always;"></p>

## Video Analysis
Often *observing* users can be more revealing than *asking* them.

Why video?
- Revisit scene many times
- Focus on different parts of scene
- Analysis can be done collaboratively
- Details which initially go unnoticed may come to light

*Facial expressions*, *gestures* and *body language* play an important role in how users *coordinate* activities and behaviour.

A study with *reluctant* participants is *unlikely* to yield *fruitful* results.

Informed consent
~ All pertinent aspects of what is to occur or might occur are disclosed to subject, subject should comprehend information
~ Subject is competent to make rational, mature judgment. Agreement should be voluntary, free from coercion and influence.

<p style="page-break-after:always;"></p>

a

<p style="page-break-after:always;"></p>

# Beyond Mouse and Keyboard
## Touch Interaction
FBTouch
~ Project to investigate alternative touch interfaces for image tagging in an application as Facebook
~ Served to demonstrate jQuery-like framework for web applications (jQMultiTouch)


## Smartphone as Command Support for Digital Whiteboard
![](https://i.imgur.com/YkBgZU1.png =500x)
Eyes Free vs. Classic vs. Whiteboard Popup

Eyes Free and Classic similar
- Both faster than Popup
- People also used Classic UI in eyes-free manner

--> Be aware of Carry-over Effects (within-subject study)

## Cross-Device Testing and Debugging Framework (XDTools)
- Within-subject study
- One debugging and on implementation task each with and without tools
- Participants generally preferred implementing and debugging with XDTools


## Digital Pen and Paper (EdFest 2005)

## Tilt-and-Tap

<p style="page-break-after:always;"></p>

a
<p style="page-break-after:always;"></p>

# Lightroom Case Study
## Process
1. Phone interviews and site visits
    - Two hours each
    - Determine photographer's workflow
        - timeline of recent shot
        - timeframe for parts of workflow
    - NOT about Photoshop experience
2. Card sort exercise
    1. Arrange and group task cards to represent their workflow
        - Asked to demonstrate if and how grouping could be changed
    2. Map feature cards to task groupings

### Outcome
- Ease with which many tasks and feature were discarded
- Level of consensus which tasks and feature were essential


## Questionnaires
Different kinds of questions:
- General background questions (e.g. age, gender, skills)
- Scalar questions (level of (dis-)agreement with statements)
- Multiple choice
- Open questions

### Likert Item and Scale
Likert Item
~ Value in numerical range encoding level of (dis-)agreement

Likert Scale
~ Sum of responses from a set of Likert items

<p style="page-break-after:always;"></p>

# Communication and Collaboration
### Conversation rules
1. The current speaker chooses the next speaker by asking a question, making a request or inviting an opinion
2. Another person starts speaking
3. The current speaker keeps speaking

### Irritating conversations
People who don't play by the rules and
- constantly interrupt
- don't pick up on the cues when to stop


<img src="https://i.imgur.com/SIAw1jp.png" style="float:right; width:350px"></img>

![](https://i.imgur.com/jkRDcSe.png =350x)

### Concluding remarks
- Supporting collaboration is about helping people work together and not solely about avoiding conflict
- If users are aware of what others are doing they will naturally tend to avoid conflict
- There needs to be a balance between keeping users informed and respecting privacy

