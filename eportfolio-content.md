# Task: finalise the ePortfolio pages

You are editing the portfolio site in this repo (kyawlinnthant.com, GitHub Pages).

## What to do

1. Create `pages.css`, `resume.html`, `reflection.html` and `letter.html` if they do not
   already exist (content for the three HTML pages is below; `pages.css` is already
   written and should be left as-is if present).
2. Replace the body content of `reflection.html` and `letter.html` with the final text
   in this file.
3. Delete every `<div class="draft">…</div>` block across all three pages. When you are
   done, `grep -c 'class="draft"' resume.html reflection.html letter.html` must return 0
   for all three.
4. In `index.html`, update the nav to include the three new pages and make the logo a
   link home:

```html
    <li><a href="#about">About</a></li>
    <li><a href="#projects">Work</a></li>
    <li><a href="resume.html">Resume</a></li>
    <li><a href="reflection.html">Reflection</a></li>
    <li><a href="letter.html">Letter</a></li>
    <li><a href="#contact">Contact</a></li>
```

```html
  <a href="index.html" class="nav-name" style="text-decoration:none;color:inherit">KLT<span class="nav-dot">.</span></a>
```

5. In `index.html`, insert the extra About paragraph below into the `.about-right` div,
   after the existing two paragraphs.
6. Check the nav wraps cleanly at 390px width. If it crowds, shorten the labels to
   `Resume` / `Reflect` / `Letter`.

Keep all existing classes and styling. Do not restyle anything.

---

## 1. index.html — extra paragraph for the About section

Insert as a third `<p>` inside `.about-right`:

```html
<p>What keeps me on the hardware side is that the measurement is only as good as the
circuit underneath it. A respiratory device either resolves the thing a clinician needs
to see or it does not, and that comes down to sensor choice, timing and signal chain
rather than anything downstream. I like problems where the answer is physical and
testable.</p>
```

---

## 2. reflection.html — final content

Replace everything inside `<main class="doc">` with this:

```html
  <p class="qn">What were your expectations about your internship before you joined?</p>

  <p>I expected to be given defined tasks inside a project someone else was directing. My
  picture of a twelve-week internship was that I would support an existing build, learn the
  tools the team already used, and hand work back to an engineer who would decide whether it
  was right. I assumed the significant design decisions would already have been made, and
  that my job was to execute against them carefully.</p>

  <p>I also assumed the clinical side of the project would sit with someone else. I thought
  the requirements would arrive already translated into engineering terms, and that my
  contact with the hospital would be limited to seeing the device used at the end, if at
  all.</p>

  <p class="qn">What was the reality? How was it different from your expectations?</p>

  <p>I was made team lead on the hardware build, and the project was further from finished
  than I expected. The Forced Oscillation Technique attachment had been worked on before I
  arrived without a working unit coming out of it, so there was material to read but no
  device to improve. That inverted what I had assumed. Instead of executing decisions, I was
  making them, starting with which sensors to use and how to acquire from them.</p>

  <p>The clinical contact was also direct rather than filtered. I worked with staff at the
  Respiratory Investigation Unit at Royal North Shore Hospital and the Woolcock Institute,
  and the requirements arrived as clinical problems rather than specifications. The device
  needed to help titrate ventilator pressure for COPD patients, and turning that into an
  oscillation frequency, a pressure amplitude and an acceptable measurement error was part
  of my work, not something handed to me.</p>

  <p>The other difference was pace, and specifically the way hardware removes the option of
  iterating your way out of a problem. Board fabrication takes time, so a schematic has to
  be committed before there is any physical board to test it on. A mistake caught after
  ordering costs a week or more. That forced a discipline I had not needed at university,
  where a wrong answer can be corrected the same afternoon.</p>

  <p class="qn">What lessons were the most important from your internship? Why were they
  important?</p>

  <p>The most important technical lesson was to specify the measurement before selecting
  the parts. The device derives respiratory impedance from the phase relationship between
  airway pressure and flow, which means the two channels have to be captured at the same
  instant. The straightforward option, and the one used in the reference material available
  to me, was an on-board I²C ADC. That converter reads its input channels in sequence rather
  than together, so it puts a small timing offset between the pressure and flow samples. In
  most applications that offset is irrelevant. In this one it lands directly on the quantity
  the device exists to measure, because a skew between the channels appears as a phase error
  and therefore as an impedance error.</p>

  <p>I moved the front end to analog simultaneous sampling instead, with the DAC output and
  dual ADC acquisition phase-locked to a hardware timer interrupt at 420 Hz, and selected
  dual Honeywell HSC sensors for airway pressure and flow. The decision was not difficult
  once the measurement was written down properly. It was only difficult while I was thinking
  about the converter as a component choice rather than as part of the measurement. That is
  the lesson I took: work out what the instrument has to resolve first, then choose hardware
  that serves it, rather than accepting a reference design because it is the well-trodden
  path.</p>

  <p>The second lesson came from the clinicians, and it changed how I understood the point
  of the work. Talking to the respiratory staff made it clear that a measurement is only
  valuable if it answers a question someone is actually asking at the bedside. It is
  possible to build a device that measures something accurately and is still useless,
  because the number it produces does not inform a decision. That reframed the project for
  me. The specification was not a technical target I had been set; it was a clinical need I
  had to understand well enough to represent in hardware.</p>

  <p>Both lessons point the same way. Engineering judgement is mostly about understanding
  the problem precisely enough that the technical choice becomes obvious, and the work of
  getting to that understanding is not separate from the engineering. It is the
  engineering.</p>

  <p class="qn">After your workplace experience, what would you say your value proposition
  would be to an employer? How can you demonstrate this?</p>

  <p>I can take a clinical measurement requirement and turn it into working hardware, and I
  can talk to the clinicians while doing it.</p>

  <p>The evidence for the first half is the prototype. I delivered a working unit on a
  project that had not produced one before, and I built it across the full stack rather than
  one layer of it: sensor selection, a 2-layer FR-4 board of 26 by 40 mm taken from
  schematic capture through routing to Gerber export and passed design rule check with zero
  errors, firmware on an Arduino Uno R4 using a hardware-timer ISR for phase-locked
  acquisition, SolidWorks models for the tubing junctions and enclosure toleranced for print
  shrinkage, and a Flask dashboard with FFT and phase-sensitive detection to recover the
  impedance. The ADC decision is the clearest single example of engineering judgement in
  that work, because it shows the measurement driving the architecture rather than the
  other way around.</p>

  <p>The evidence for the second half is that the specification came out of conversations
  with the Respiratory Investigation Unit and the Woolcock Institute rather than out of a
  document. I taught myself PCB design during the build, which is also worth stating
  plainly: the useful thing is not that I already knew EasyEDA, but that I could get to a
  fabricable board inside a twelve-week window while the rest of the project continued.</p>

  <p class="qn">How did your internship influence the type of role in which you are
  interested?</p>

  <p>It pushed me towards medical device engineering on the hardware side, and away from
  roles that sit further from the instrument. What I found satisfying was the part where a
  design decision has a physical consequence you can measure, and where being wrong shows up
  in the data rather than in an opinion. Working on a device intended for COPD patients also
  made the clinical proximity matter to me more than I expected it to.</p>

  <p>Concretely, I am looking for a graduate role in device development, sensing or embedded
  systems in a medical context, ideally somewhere the engineering team has contact with the
  clinicians using the product. I would also take a role in test, verification or field
  support in the same domain, because both put you close to how devices behave in real use,
  and that is the knowledge I most want to build next.</p>

  <div class="page-nav">
    <a href="index.html">Home</a>
    <a href="resume.html">Resume</a>
    <a href="letter.html">Cover letter</a>
  </div>
```

---

## 3. letter.html — final content

Replace everything inside `<main class="doc">` with this:

```html
  <p><strong>Kyaw Linn Thant</strong><br>
  Sydney, NSW · kyawlinnthant55@gmail.com · linkedin.com/in/kyaw-linn-thant</p>

  <p>Dear Hiring Manager,</p>

  <p>I am writing to apply for a Graduate Engineer position in the Industrus Engineering
  Graduate Program. I am a final-year Biomedical Engineering (Honours) student at the
  University of Technology Sydney, graduating in December 2026 with a WAM of 84. Over a
  twelve-week internship at Optik Consultancy I led the hardware build of a Forced
  Oscillation Technique attachment for CPAP and BiPAP machines, developed for the
  Respiratory Investigation Unit at Royal North Shore Hospital and the Woolcock Institute of
  Medical Research, and delivered the first working prototype the project had produced. I
  address each of the selection criteria below.</p>

  <div class="crit">
    <h3>3.1 — A commitment to ethical conduct and the highest standards of professional
    accountability</h3>
    <span class="star-key">Situation</span>
    <p>The device I built was intended to inform ventilator pressure decisions for COPD
    patients, so any number it produced would eventually carry clinical weight. When the
    first assembled board came back and began returning impedance values, those values
    looked reasonable.</p>
    <span class="star-key">Task</span>
    <p>Reasonable is not the same as verified. I had to decide whether to report the device
    as working on the strength of plausible output, or to establish what it had actually
    been shown to do before saying anything to the client.</p>
    <span class="star-key">Action</span>
    <p>I checked the output against known conditions on the bench before reporting it,
    rather than treating a plausible reading as a validated one. When I did report to the
    team and the clinical stakeholders, I was explicit about the boundary: what had been
    demonstrated on the bench, what remained uncalibrated, and that nothing had been
    validated against a clinical reference. I raised the limitations myself rather than
    waiting to be asked.</p>
    <span class="star-key">Result</span>
    <p>The prototype was handed over with an accurate account of its state, which meant the
    clinical partners could judge what it was ready for. I would rather deliver a device
    with its limitations documented than one that is believed to be further along than it
    is, particularly where the eventual user is a patient.</p>
  </div>

  <div class="crit">
    <h3>3.2 — Demonstrated ability to effectively communicate both with other engineers and
    with stakeholders from different fields</h3>
    <span class="star-key">Situation</span>
    <p>The requirements for the device came from respiratory clinicians and researchers at
    Royal North Shore Hospital and the Woolcock Institute, who described what they needed
    clinically rather than in engineering terms.</p>
    <span class="star-key">Task</span>
    <p>I needed to turn a clinical need, helping titrate ventilator pressure for COPD
    patients, into quantities I could build against: oscillation frequency, pressure
    amplitude, sensor range and acceptable measurement error. I also needed the clinicians
    to understand one technical constraint well enough to accept its consequences, because
    it shaped the hardware.</p>
    <span class="star-key">Action</span>
    <p>I asked about how the measurement would be used at the bedside rather than what
    specification they wanted, which is what produced the numbers I needed. When explaining
    why the acquisition architecture mattered, I left the converter details out and put it in
    terms of the measurement: pressure and flow have to be read at the same moment, because
    the timing between them is the signal, and reading them one after the other introduces an
    error that looks like a real change in the patient. Within the engineering team I
    described the same decision in terms of sequential I²C reads and phase skew.</p>
    <span class="star-key">Result</span>
    <p>The specification I worked to came out of those conversations rather than being
    assumed, and the sensor ranges were chosen against real clinical values. Adjusting the
    level of technical detail to the audience is the part I would carry into a consulting
    environment, where the same decision often has to be explained several different ways.</p>
  </div>

  <div class="crit">
    <h3>3.3 — The ability to engage with a creative, innovative and proactive
    environment</h3>
    <span class="star-key">Situation</span>
    <p>The device measures respiratory impedance, which is derived from the phase
    relationship between airway pressure and flow, so the two channels must be sampled at the
    same instant.</p>
    <span class="star-key">Task</span>
    <p>The straightforward path was an on-board I²C ADC, as the reference designs available
    to me used. That converter reads its input channels sequentially rather than together,
    which introduces a timing skew between the pressure and flow samples and corrupts the
    phase measurement the device exists to make. I had to decide whether to accept the
    standard architecture or change it.</p>
    <span class="star-key">Action</span>
    <p>I specified the measurement first and then selected hardware to serve it, moving to
    analog simultaneous sampling with the DAC output and dual ADC acquisition phase-locked to
    a hardware timer interrupt at 420 Hz. I selected dual Honeywell HSC sensors for airway
    pressure at ±10 inH₂O and flow at ±2 inH₂O, designed the analog conditioning around them,
    and took the 2-layer FR-4 board from schematic through routing to Gerber export, passing
    design rule check with zero errors before fabrication at JLCPCB.</p>
    <span class="star-key">Result</span>
    <p>The build produced the first working prototype the project had achieved, after
    previous teams had not delivered one. Both channels remain phase-coherent, which is what
    makes the impedance measurement valid rather than merely plausible.</p>
  </div>

  <div class="crit">
    <h3>3.4 — Demonstrated ability to use and manage information</h3>
    <span class="star-key">Situation</span>
    <p>The project had been attempted before I joined, so I inherited partial material from
    earlier work, and the component selection required comparing options across several
    manufacturers' datasheets.</p>
    <span class="star-key">Task</span>
    <p>I needed to establish what was reusable from the earlier work and what was not, choose
    sensors and an acquisition path on the evidence rather than on convenience, and document
    my own build so that the next person did not face the same problem I had.</p>
    <span class="star-key">Action</span>
    <p>I worked through the prior material first and separated conclusions that were
    supported from assumptions that had been carried forward untested, which is where the
    on-board ADC assumption surfaced. For the sensors I compared pressure ranges, output
    types and interface options across datasheets against the clinical values I had, which
    led to the dual Honeywell HSC selection. Through the build I kept the schematic, layout
    and Gerber versions ordered alongside notes on why each decision was made, and used the
    Flask dashboard to log acquisition data so results could be reviewed rather than
    recalled.</p>
    <span class="star-key">Result</span>
    <p>The design decisions are traceable to the reasoning and the datasheet evidence behind
    them, and the project was handed on in a state where someone else could pick it up.
    Having been on the receiving end of an undocumented handover, I treat documentation as
    part of delivering the work rather than something after it.</p>
  </div>

  <div class="crit">
    <h3>3.5 — The ability to manage your own performance in a professional environment</h3>
    <span class="star-key">Situation</span>
    <p>I had twelve weeks, a project that previous teams had not completed, and no prior
    experience with PCB design, which the build required.</p>
    <span class="star-key">Task</span>
    <p>I had to sequence the work around a hard external constraint. Board fabrication has a
    lead time, so the schematic and layout had to be finished and committed well before the
    end of the internship, or there would be no physical board to test and nothing to hand
    over.</p>
    <span class="star-key">Action</span>
    <p>I worked backwards from the fabrication date. Sensor selection and the acquisition
    decision came first because everything downstream depended on them, then schematic
    capture and layout, and I taught myself EasyEDA Pro against that deadline rather than
    learning it in the abstract. While the board was in fabrication I moved onto work that
    did not depend on it, the SolidWorks tubing and enclosure components and the firmware, so
    the wait was not idle. When something did not behave as expected I checked it against the
    datasheet and the measurement I was trying to make before changing anything, rather than
    adjusting the design until the symptom disappeared.</p>
    <span class="star-key">Result</span>
    <p>The board was fabricated, assembled and tested inside the internship, and the
    prototype was delivered. Planning around the constraint I could not move, rather than
    around the order I would have preferred to work in, is the habit I took from it.</p>
  </div>

  <div class="crit">
    <h3>3.6 — A demonstrated ability to work as part of a team and to show leadership when
    required</h3>
    <span class="star-key">Situation</span>
    <p>I led the hardware side of the build as part of a wider project team, with the work
    spanning electronics, firmware, mechanical components and the software dashboard, all
    against the same deadline.</p>
    <span class="star-key">Task</span>
    <p>The work had to be divided so that people were not blocked waiting on each other, and
    the acquisition architecture decision had to be made early because the rest of the
    hardware depended on it.</p>
    <span class="star-key">Action</span>
    <p>I sequenced the work around the dependencies rather than around equal division,
    settling the sensor and acquisition decisions first so that the mechanical and firmware
    work could proceed in parallel against fixed interfaces. Departing from the reference
    design was a call I had to make and then justify, so I explained the phase skew problem
    to the team rather than asserting the conclusion, on the basis that a decision people
    understand is one they can build on and challenge. Where I was the one who had read the
    datasheets I said so, and where someone else knew the ground better I deferred.</p>
    <span class="star-key">Result</span>
    <p>The separate strands came together into a working prototype within the twelve weeks.
    What I took from leading it is that the useful part of the role was removing ambiguity
    early, because most of the time that was lost on this project before I joined was lost to
    decisions that had not been made rather than to work that was difficult.</p>
  </div>

  <p style="margin-top:38px">I would welcome the opportunity to discuss how my hardware
  experience and clinical engineering background could contribute to the Industrus Graduate
  Program. My resume and a fuller account of the respiratory device are available on this
  site.</p>

  <p>Regards,<br>Kyaw Linn Thant</p>

  <div class="page-nav">
    <a href="index.html">Home</a>
    <a href="resume.html">Resume</a>
    <a href="reflection.html">Internship reflection</a>
  </div>
```

---

## 4. resume.html — remove the draft boxes only

The resume content is already final. Delete the three `<div class="draft">` blocks and
leave everything else unchanged.

---

## 5. Deploy

```
git add .
git commit -m "Finalise ePortfolio: resume, reflection and cover letter pages"
git push
```

Hard refresh after a minute or two.
