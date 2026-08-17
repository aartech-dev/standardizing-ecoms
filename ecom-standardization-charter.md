# Standardizing the Slot Car Brushless Electronic Commutator (eCom)

### A discussion paper and draft charter for BSCRA, ISRA, affiliated national bodies, eCom developers, manufacturers, and racers

*Version 0.1 — draft for community comment. This paper does not represent a ratified position of any organization named in it. It exists to frame the debate accurately enough that a working group can reach consensus, not to pre-decide the outcome.*

---

## 1. Purpose of this paper

Brushless motors and the electronic commutators (eComs) that drive them have moved, in the space of about four years, from a garage experiment to a technology present in a majority of cars at the top level of international competition. That speed of adoption has outrun the governance structures — rules, scrutineering procedures, and licensing norms — that slot car racing built around brushed motors over the preceding six decades.

This paper has three goals:

1. **Record the history** accurately enough that newcomers to the debate understand why the community is split into open and closed camps, and why that split is not simply a matter of taste.
2. **Lay out the open-vs-closed tradeoff** — technical, commercial, and competitive — without pretending one side is obviously right.
3. **Propose a working model** — a published design spec with independent, multi-vendor homologation against it — that a technical working group under BSCRA and ISRA could adopt, and pose the open questions that group still needs to resolve.

Nothing here is a rule. It is an attempt to get everyone arguing about the same facts.

---

## 2. Background: what an eCom actually is, and why it's a governance problem

A conventional slot car motor is a brushed DC motor: apply a voltage, current flows through the brushes and commutator, the motor turns proportionally to that voltage, and the driver's controller *is* the throttle. A brushless motor has no brushes — it needs an external circuit to sequence current through its three phases, sense or estimate rotor position, and translate the driver's track voltage into the right commutation pattern. That circuit is the **eCom** (electronic commutator), directly descended from the electronic speed controllers (ESCs) used in radio-control aircraft and drones, but adapted to work transparently with a standard analog slot car hand controller rather than an RC-style PWM throttle signal.

This is the governance problem in one sentence: **a brushed motor is a passive, fully-characterized piece of copper and magnets that scrutineers can measure directly; an eCom is a piece of firmware that decides, in software, how the car behaves.** Everything downstream in this paper — the standardization argument, the open/closed argument, the scrutineering argument — follows from that difference. Two eComs built on identical silicon can behave completely differently depending on what code is running on them, and unlike a motor's winding count or magnet grade, firmware is not something you can check with a vernier caliper.

---

## 3. History (2021–2026)

**2021 — Budge / Mack / Smits (the "SBM" origin).** Bob Budge and Richard Mack were the first to campaign brushless-powered cars competitively at a level close to brushed pace, with Budge reaching national finals. The boards they raced were designed by Peter Smits, a Canadian electronics designer whose background was in ESCs for multirotor drones, adapted specifically for slot racing so the car could still be driven from a normal analog hand controller rather than an RC throttle stick. This SBM effort is the common ancestor of essentially everything that followed: it proved the concept, and it set the template — **closed hardware, with firmware that is semi-open in the sense that Smits has engaged with the community on behavior and tuning, but the boards themselves are not open for cloning or independent audit.**

**2022–2023 — Remora (open source).** A parallel open-source effort produced the Remora eCom, publishing full KiCad hardware designs and pairing them with **ESCape32**, an open-source firmware also used elsewhere in the small-electric-motor hobby world. Remora's own framing of its licensing is worth quoting because it captures the nuance that "open source" doesn't mean "unencumbered": the designs are described as *"open but not free"* — released under a modified Creative Commons license that lets anyone build their own board from the published files, but reserves the IP, expects commercial sellers to negotiate royalties with the originators, and excludes military use. This is **Remora 1**: schematics, PCB layout, bill of materials, and firmware all public.

**Remora 2** narrowed what "open" means in a commercially useful direction. Rather than publishing full design-tool source files (which lets anyone become a de facto manufacturer with zero investment), Remora 2 publishes **Gerber/manufacturing files** — enough for anyone to get boards fabricated and populated — while formalizing a **licensing path for commercial sellers**. The distinction matters: Remora 1's approach maximizes hobbyist reproducibility and transparency at the cost of making it hard for any single seller to build a sustainable business around the design; Remora 2 tries to keep the transparency (you can still see exactly what the board is and verify it against the Gerbers) while giving legitimate manufacturers a licensed, revenue-supporting route to sell finished, warrantied product.

**Infinity ESC (Jan Thoen) — what openness enables.** Because Remora's hardware is open, it became possible for a third party to build an entirely independent firmware stack for it without needing anyone's permission or cooperation. Jan Thoen, working in collaboration with AART, developed [**Infinity ESC**](https://gitlab.com/slotracing/infinity-racing/esc) — a modular C++20/STM32 firmware platform delivering field-oriented control (FOC), telemetry, and richer communication layers, while remaining compatible with existing ESCape32-based hardware. Infinity ESC did not require Remora's designers to build it, review it, or ship it; it exists because the hardware was open enough for someone outside the founding team to pick it up and take it somewhere the original authors hadn't planned to go. This is the clearest evidence in this history for the strongest argument in favor of openness: at this early a stage in brushless eCom development, nobody yet knows which firmware architecture, control strategy, or feature set will turn out to be right, and an open hardware base lets multiple independent teams find out in parallel rather than betting the whole community on one closed team's roadmap. That is a genuine, not merely ideological, advantage — though, as with the rest of this paper, it has to be weighed against the commercial and scrutineering costs already discussed.

**Latslot (closed).** A further, fully closed effort — hardware and firmware both proprietary, no published design files, sold as a finished product with no path for independent reproduction or code audit. Whatever the technical merits, from a governance standpoint Latslot represents the opposite pole from Remora 1: a black box that racers and scrutineers must trust rather than verify.

**2023–2024 — ISRA Eurosport adoption.** Brushless power moved from experimental to dominant at the top of international competition strikingly fast: roughly 60% of cars at the 2023 ISRA Worlds (New Jersey) ran brushless, and by the 2024 ISRA Worlds (Italy) essentially the entire field, in both 1/24 and 1/32, was brushless. This is the point at which "brushless eCom standardization" stopped being a hobbyist curiosity and became an urgent governance question for the sport's top category, because a technology that determines the outcome of a world championship cannot be left unscrutineered.

**2025–2026 — BSCRA Intro-class adoption: a live single-vendor mandate.** BSCRA's members voted to permit brushless motors in the Intro 32 class, and mandated a specific pairing: the **MidAmerica 3000Kv** motor with the **MidAmerica Trixie 2** eCom. Trixie 2 — designed by AART, the same team behind Remora 1 and Remora 2 (see below) — was built in close cooperation with BSCRA, targeted initially at BSCRA's Genesis racing program and tuned specifically to work well with the 3000Kv motor. Unlike Remora, Trixie 2 is **completely closed and single-sourced**, with the microcontroller fully locked at RDP Level 2 (see §7) and no published design files, firmware, or manufacturing route for anyone else to build one.

Intro 32 is therefore a live instance of the single-vendor model discussed in §5: one approved motor, one approved eCom, one supplier for each, which in principle makes "did you meet spec" trivial to check by part number alone. But because Trixie 2 ships RDP2-locked, confirming that a given board on a given car is actually running its approved, motor-tuned firmware — rather than a modified, substituted, or simply different build in an identical-looking case — still requires an external, behavioral check (§6–7), not a code read. Intro 32 is a useful test case for the homologation approach this paper proposes precisely because it shows that model is still needed even where the open/closed and single-vendor questions have already been decided: BSCRA doesn't need this paper to choose a vendor for Intro 32, but it does need a scrutineering method that works given the choice it already made.

**AART: one developer spanning the whole spectrum.** Trixie 2 is worth pausing on for a second reason. Where Remora is open, Trixie 2 is the opposite — and both come from the same team. AART has separately developed a comparable board for Slot.it — closed today, but designed with the explicit possibility of being open-sourced later.

This matters for the standardization debate because it undercuts the assumption that "open" and "closed" map onto fixed ideological camps or competing companies. AART has, within a few years, shipped a fully open design (Remora 1), an open-manufacturing/licensed-commercial design (Remora 2), a fully closed single-source design built to a specific governing body's brief (Trixie 2), and a closed-but-potentially-opening design (the Slot.it board). The model tracks the commercial context of each engagement — a hobbyist community project, a licensed product line, a bespoke commission for a national body's control-part program — not a settled belief about which model is "correct." That is direct evidence for the position this paper takes in §6: **the standard should regulate behavior, not business model**, because even a single developer is demonstrably willing to work under any of them depending on who is asking and what they need.

**2025–2026 — BSL: two approved eComs, one un-level playing field.** A similarly structured body, BSL, took a different path from BSCRA's single-vendor mandate: rather than naming one eCom, it has permitted two for this season — the Do-Slot (white) and the MidAmerica Trixie 2. Oscilloscope-based testing and analysis by racers has since found that the two boards are timed differently: **Trixie 2 runs with greater commutation advance and a correspondingly higher potential RPM ceiling** than the Do-Slot unit.

This is exactly the failure mode §5 warns about, already happening rather than hypothetical: a rulebook that names two "approved" parts by brand rather than by measured behavior has, in effect, sanctioned two different performance envelopes under one label. Two racers can each be legally compliant with "an approved eCom" while one is running a meaningfully faster setup than the other — precisely the scenario behavioral homologation (§6) is designed to prevent, because it would flag the differing advance/RPM ceiling as an out-of-tolerance divergence regardless of which brand nameplate is on the board. It is also a present-day demonstration of why §7's argument for external, oscilloscope- or tachometer-based verification matters over a simple approved-brand list: the difference here was only caught because racers went and measured it themselves, not because the approval process was built to catch it.

**2026 — AART and Latslot: an open-source/closed-protocol compromise.** In pursuit of interoperability, AART approached Latslot to obtain the specification of the UART protocol used by Latslot's own programmer tools to adjust an eCom's commutation timing and toggle its braking behavior. AART's goal was to patch ESCape32 — the open-source firmware behind Remora — so that Remora-based boards could be configured with the same class of programmer tooling. This ran straight into a licensing conflict: ESCape32 is released under a copyleft-style open-source license that requires modifications to also be published as open source, but Latslot's protocol is proprietary, and disclosing it in full inside an open-source patch would have handed away IP Latslot has never published. Latslot initially declined for exactly this reason. After further discussion, the two parties reached a compromise: AART could publish the resulting patch as open source, satisfying ESCape32's license, provided it did not explicitly disclose the details of Latslot's protocol or implementation.

This is worth recording for two reasons. First, it is a second concrete instance — alongside AART's Remora/Trixie 2/Slot.it portfolio — of open and closed participants finding a working arrangement rather than treating the divide as absolute; the working group should not assume open-source licensing and closed-vendor IP are structurally incompatible — here, negotiated, they weren't. Second, it is a direct illustration of why field-configurable timing advance and braking behavior — exactly the parameters at issue in the BSL divergence above — cannot be fully captured by checking what left the factory: if a UART programmer can retune an eCom's advance or braking after homologation, "as shipped" compliance and "as raced" compliance are not the same question, and only a trackside behavioral check (§6–7) catches the difference regardless of which vendor's programmer made the change.

**The scrutineering proposal (this repository).** In parallel, a proposal (see `scrutineering.pdf` in this repository) for a trackside measurement device — a Hall-effect tachometer that measures unloaded motor speed at two or three fixed supply voltages to derive the motor/eCom combination's back-EMF constant, **Kₑ** — was put forward specifically to solve the "how do we scrutineer a black box quickly" problem without needing to read firmware at all. Its central insight is important enough to restate here: because eCom and motor winding resistance are small relative to total circuit resistance, and because **Kₑ = Kₜ** in consistent SI units, a simple speed-vs-voltage measurement is sufficient to catch an eCom/motor combination that is out of spec, regardless of what code is running inside it, or whether that code can be read at all. This is the technical key that makes the "closed firmware is fine as long as behavior is bounded and externally measurable" position workable rather than wishful — see §6.

There is also a commercial opportunity here worth making explicit rather than leaving implicit. If the tachometer/Kₑ device itself is published as an open reference design — schematic and Gerbers, in the Remora 2 mould — any manufacturer, hobbyist or commercial, can build and sell their own motor/eCom tester without needing anyone's permission. That heads off a problem the model in §6 would otherwise create by itself: a scrutineering process that depends on one piece of measurement equipment is only as robust as that equipment's supply chain, and a single-vendor tester would recreate exactly the single-point-of-failure risk this paper flags for eComs in §5. An open design instead gives clubs, national bodies, and individual racers who want to self-check before an event a real, competitively-priced product to buy — and gives manufacturers the same "open but not free" licensing path already proven by Remora a second product line to build a business around: a motor/eCom tester, not just an eCom. It's also likely to need to grow beyond a simple tachometer: the PWM-level parameters in §6's spec (sync ramp rate, duty-cycle limits, switching frequency, drag-brake duty cycle) aren't visible in a speed-vs-voltage curve and need waveform capture to verify — closer to a purpose-built, low-cost oscilloscope channel than a tachometer. That's a more capable, more valuable product to build and sell, not a reason to abandon the idea.

---

## 4. Open vs. closed: the real tradeoff

It is tempting to frame this as open-source idealism versus corporate self-interest. That framing is wrong and will poison the working group's consensus-building if it takes hold. Every position in this space is a rational response to a real constraint.

| | **Remora 1**<br>(fully open) | **Remora 2**<br>(open mfg. files + licensed commercial builds) | **SBM**<br>(closed hardware, semi-open firmware engagement) | **Latslot**<br>(fully closed) | **Trixie 2**<br>(AART, fully closed, single-source) |
|---|---|---|---|---|---|
| Hardware design files | Full schematic + PCB source published | Gerbers/manufacturing files published; source files not | Proprietary, unpublished | Proprietary, unpublished | Proprietary, unpublished |
| Firmware | Open source (ESCape32) | Open source (ESCape32) | Closed; behavior/tuning discussed publicly by the designer | Closed, no public disclosure | Closed, RDP Level 2 locked |
| Who can build one | Anyone with a fab and the files | Anyone with a fab; commercial resale expects a license | Only the originating team / licensees | Only the manufacturer | Only AART, by commission |
| Independent audit possible | Yes, fully | Yes, board-level (matches Gerbers); firmware is open | No | No | No |
| Revenue model | Voluntary royalty on commercial use; weak enforcement | Licensed commercial manufacture; stronger enforcement | Direct hardware sales, margin-funded | Direct hardware sales, margin-funded, IP as moat | Commissioned single-source supply for a specific program (BSCRA/Genesis) |

Notably, Remora 1/2 and Trixie 2 come from the same developer (AART) — see the history note above. The table is best read as a spectrum of *models one team is willing to build*, not a lineup of competing philosophies.

### Pros and cons of open designs

**Pros:**
- Fully scrutineerable in principle — anyone, including a rival manufacturer or a suspicious competitor, can check what the board actually does.
- Low barrier to entry drives price down and lets the hobbyist/DIY culture of slot racing continue to participate in the technology, not just consume it.
- Community-driven bug fixing and feature development (ESCape32 is used well beyond slot racing, so it benefits from a bigger contributor base than any single vendor could fund).
- Not just in theory: Jan Thoen's independently-developed [Infinity ESC](https://gitlab.com/slotracing/infinity-racing/esc) firmware (FOC control, telemetry) exists specifically because Remora's hardware is open — a concrete case of a third party extending the ecosystem without needing the original team's permission.
- No single point of commercial failure: if one seller stops trading, the design survives and others can build it.

**Cons:**
- Weak commercial incentive to invest in tooling, QC, support, and warranty — "open but not free" licensing is difficult to enforce against a small manufacturer overseas, so the people doing the engineering work often capture little of the value.
- Fragmentation risk: many small-batch builders of "the same" open design, with real-world variance in component sourcing, assembly quality, and firmware version, which is itself a scrutineering headache (see §5) even though the *design* is open.
- No natural single point of accountability if something fails on-track (fire, injury) — liability is diffuse.
- Ironically, full openness can make the "single vendor, easy scrutineering" goal (§5) *harder*, not easier, because there is no one throat to choke and no one bill-of-materials to certify.

> **Why openness matters right now.** Whatever the long-run commercial tradeoffs, the working group should weigh one more factor: this technology is still young. Infinity ESC's existence — a materially different firmware approach (FOC control, telemetry) built by someone outside the founding team, made possible only because the hardware was open — is a live demonstration that openness accelerates the exploration phase of a new technology. A standard that forecloses this kind of experimentation before the community has learned what a mature eCom architecture even looks like risks optimizing for stability before there is anything proven worth stabilizing.

### Pros and cons of closed designs

**Pros:**
- A funded, accountable business behind the product: warranty, consistent QC, one company to hold responsible for defects or misbehavior.
- Commercial incentive to invest in R&D — closed IP is how the designer captures return on the (real) engineering effort of getting brushless commutation to behave transparently under an analog slot car controller, something that took Smits real drone-ESC expertise to solve.
- Simpler supply chain for race organizers to reason about: fewer variants, easier to write a single rule ("must be a genuine SBM/Latslot unit, unmodified").
- No risk of the design being commoditized into a race-to-the-bottom on price that starves out quality control.

**Cons:**
- Zero independent verifiability. The community's only assurance that the firmware isn't doing something outside the rules (traction control, artificial launch control, an undisclosed "race mode") is the manufacturer's word.
- Single point of commercial failure: if the manufacturer stops trading or stops supporting a product line, owners are stuck with unrepairable, unmodifiable hardware.
- Monopoly pricing risk in a small hobby market with real switching costs (a control system tied to a specific class rule).
- Firmware updates are opaque — a manufacturer could silently change behavior between events (see the OTA/re-homologation discussion in §7) with no external record of what changed.
- **Externally indistinguishable, internally different.** Two physically identical locked boards can be running completely different firmware builds, versions, or configurations — there is no way for a buyer, a scrutineer, or even the vendor's own downstream support staff to verify what is actually flashed on a specific unit without trusting the vendor's own paperwork. Behavioral homologation (§6–7) mitigates this for *competition legality* — if measured performance is in-spec, it doesn't matter what's inside — but it does nothing to protect a racer's more basic interest in knowing what they actually bought.
- **Locked hardware is effectively obsolete the moment a bug-fixed or improved firmware version ships**, unless the vendor has built a signed field-update path. RDP-level protection forecloses reflashing over the debug interface by design (§7); if there is no alternative update mechanism, a racer who wants the fix has to buy a new unit rather than update the one they already own. Where cost is a barrier — which, for a hobby sport, it usually is — this turns ordinary firmware maintenance into a recurring hardware tax tied to a single vendor, a cost that open firmware (which any owner can reflash themselves, for free) does not impose.

These two problems compound each other: because locked units are externally indistinguishable, there is no way for a racer to tell, by looking at the board they're offered, whether it's running the latest fix or a year-old build — so there's no way to even shop around for "the updated one." The only lever available to the community is disclosure: requiring the homologation register (§6) to record firmware version or hash per submission, and — where practical — requiring a visible, human-readable version marking on the unit itself (a printed batch or date code matching the register), so that at least the *existence* of multiple firmware versions in the field is visible, even when the code behind them isn't.

### Commercial viability and business models

None of the models above has a business model that is obviously sustainable at hobby-market volumes, and the working group should not pretend otherwise:

- **Fully open (Remora 1)** relies on voluntary compliance with a royalty request that has no realistic enforcement mechanism against small-scale or overseas builders. It sustains itself on goodwill and the originators' willingness to keep contributing without being paid proportionally to the value created — a pattern familiar from open-source hardware generally, and one that tends to produce burnout rather than a durable supply of product.
- **Open manufacturing files + licensed resale (Remora 2)** is a more conventional attempt at a sustainable model — closer to how, say, an open instruction-set architecture can still support licensed silicon vendors. It only works if the governing bodies are willing to give "properly licensed" builders some recognition (see §6) that unlicensed clones don't get, which is itself a policy choice this paper flags as needing consensus.
- **Closed hardware, semi-open engagement (SBM)** monetizes through direct sales with the designer's expertise as the moat, and sustains itself the way any small specialist electronics business does — which is to say, it is only as durable as the business itself, with no fallback if Smits or the SBM team stop building.
- **Fully closed (Latslot)** is the most conventional business model and, all else equal, the easiest to keep funded — but it offers racers and rule-makers the least insight into what they are actually racing.
- **Commissioned single-source (Trixie 2)** sidesteps the open-market sustainability question entirely by tying the business model to a specific customer relationship — BSCRA/Genesis — rather than to retail sales. It is arguably the most commercially secure of the five for as long as that relationship holds, and the least resilient if it doesn't: there is, by design, no other supplier to fall back on.

It's also worth being honest about how much protection "closed hardware" actually buys. Reverse engineering a PCB — delayering it, tracing nets, reading component markings, and reconstructing a schematic and bill of materials — is a well-understood, comparatively low-cost exercise for a board this size and complexity; unlike firmware, which RDP can lock down at the silicon level (§7), there is no equivalent hardware-level protection available to a small manufacturer. Software reverse engineering is a smaller concern here for a different reason: with ESCape32 already open, mature, and good, there's little incentive for anyone to bother reverse engineering a closed competitor's firmware when a legitimate open alternative already exists. And even where copying the hardware is technically easy, the addressable market for slot car eComs is small enough that pursuing formal IP enforcement — cease-and-desist letters, let alone litigation — is unlikely to be economically rational for anyone involved. "Closed hardware" in this space is therefore better understood as a soft deterrent — inconvenience, lead time, and the value of buying a supported, warrantied product from the original designer — than as a hard technical or legal moat. That doesn't make it worthless, but it does mean the commercial case for closed hardware rests on relationship and service, not on unclonability, which softens — without eliminating — the tradeoff described below. AI-assisted tooling is trending this further in the same direction: schematic reconstruction from board photos, netlist extraction, and firmware disassembly/analysis are all measurably faster and more accessible than they were even a couple of years ago, and the same tools lower the barrier for a new entrant to develop a competing eCom — open or closed — from scratch. Whatever moat closed hardware and firmware once offered is only getting thinner; a standard that assumes secrecy is durable is building on ground that is actively eroding.

The uncomfortable conclusion the working group has to sit with: **the most commercially sustainable models are also the least independently verifiable ones**, and the most verifiable model is the least commercially sustainable one. Any standard that ignores this tension and simply mandates "must be open source" is likely to starve out the manufacturers who can actually deliver supported, warrantied product at race-meeting volumes. A standard that mandates "must be a specific closed product" solves supply and accountability but recreates a single-vendor monopoly and removes the community's ability to check anything.

---

## 5. The central conflict: a level playing field wants one standard; a healthy market wants many vendors

Racers' instinct is straightforward and legitimate: fair competition requires that no competitor gain an advantage from a piece of hardware whose behavior nobody else can check. Taken to its logical end, that instinct argues for **a single approved eCom** — one design, one firmware, ideally one vendor, so that "did you meet spec" reduces to "is this the approved part." BSCRA's Intro 32 class (§3) is not a hypothetical here — it is already living this choice, having mandated the MidAmerica 3000Kv motor paired with the single-sourced, closed Trixie 2 eCom.

But a single-vendor mandate has costs the sport has hit before with control tires, control motors, and control chassis in various classes: it creates a monopoly supplier with no competitive pressure on price or lead time, it puts an entire class's supply at the mercy of one company's business continuity, and — specific to *this* technology — it forecloses the open-source ecosystem entirely, which cuts against the DIY, build-it-yourself culture that has kept slot racing accessible for decades.

The two goals are genuinely in tension, not just apparently so:

- **Fair, simple scrutineering** wants: few variants, ideally one, ideally with source code or a certified reference the scrutineer can compare against, checked in minutes trackside.
- **A healthy, resilient, accessible market** wants: multiple vendors, competitive pricing, a path for hobbyists to build their own, and no single company able to hold a whole racing class hostage.

Section 6 proposes the model the working group should evaluate for reconciling these: **standardize the spec, not the vendor.**

---

## 6. Proposed model: a published design spec with multi-vendor homologation, not a single mandated product

This is the pattern used successfully by other technical standards bodies that face exactly this tension — USB-IF, the Bluetooth SIG, the Wi-Fi Alliance, Qi wireless charging: **the governing body owns and publishes a spec and a conformance test; it does not build or sell the product.** Applied here:

1. **BSCRA and ISRA (jointly, via a technical working group with cross-membership) publish a design spec — a "design point," not a reference design.** This should define the *performance envelope*, not the implementation. At minimum, it should bound:
   - permissible Kₑ/Kᵥ range for a given class;
   - timing-advance limits (see the BSL Do-Slot/Trixie 2 divergence in §3 for exactly why this needs a bound, not just a brand list);
   - startup/synchronization PWM frequency, and the rate at which duty cycle is permitted to ramp up during that sync phase — both shape how a car launches off the line, and both are exactly the kind of parameter a UART programmer (§3, AART/Latslot) can retune after homologation;
   - maximum permitted duty cycle, which caps delivered power independently of the current/thermal limits below;
   - PWM switching frequency, low and high, which affects motor heating, efficiency, and driver-perceptible behavior;
   - permitted duty-cycle range during drag braking, since brake strength is as much a competitive parameter as top speed, and just as easy to tune outside a "fair" envelope;
   - current/thermal limits;
   - mechanical footprint and connector standard;
   - permitted feature set (e.g., explicitly prohibiting traction control, launch control, telemetry-linked driver aids, or any behavior that varies based on detected competitors); and
   - required safety behavior (brownout/low-voltage cutoff, fail-safe on signal loss).
2. **Any manufacturer — open-source hobbyist, licensed Remora 2 builder, SBM, Latslot, or a newcomer — may submit an implementation for homologation** against that spec. Being closed-source is not disqualifying; being open-source confers no shortcut either. The spec is implementation-agnostic by design, which is what makes it possible for Principle 4 below to hold.
3. **Homologation is behavioral, not code-based.** This is where the scrutineering proposal in this repository does real work: a bench test (and a fast trackside spot-check using the same Hall-effect/Kₑ method) verifies what the unit *does* — speed versus voltage, current draw, thermal behavior — without anyone needing to read anyone's firmware. This sidesteps the open/closed fight almost entirely for the purposes of competition legality: **the rulebook cares about the transfer function, not the source code.** The measurement device itself should be published as an open reference design (§3) so the testing equipment doesn't become its own single-vendor bottleneck — and so building and selling motor/eCom testers becomes a real commercial opportunity in its own right.
4. **A public register** of homologated eComs is maintained by BSCRA/ISRA — make, model, firmware version (where applicable, by hash or version string, not source), and the measured envelope — so competitors, organizers, and scrutineers all work from the same reference.
5. **Firmware changes require re-homologation.** A unit whose measured behavior drifts outside its registered envelope — whether from a firmware update, a hardware revision, manufacturing drift, or a field configuration change made via a vendor's own programmer/tuning tool (see the AART/Latslot UART protocol case in §3) — is no longer homologated until re-tested. Practically, this argues for either disabling field firmware updates and reconfiguration during a homologation period, or requiring updates to be versioned and re-submitted (see §7). Critically, it also means "as shipped" compliance is not enough: because timing advance and braking are field-adjustable on multiple vendors' hardware, trackside behavioral checks (§7) have to verify "as raced" configuration, not just the factory default.

This model explicitly protects both open and closed manufacturers' core interests: open projects keep the right to exist and compete without being regulated out of the market by a spec that assumes proprietary tooling; closed manufacturers keep their IP, including RDP-level firmware locking, because homologation never requires disclosing source.

---

## 7. Practicalities of board locking (RDP levels 0–2) and what it means for scrutineering

Several current and prospective boards — the MidAmerica Trixie 2 (§3) is a live, concrete example, shipping today at RDP Level 2 — use STMicroelectronics-style microcontrollers, which support hardware **Readout Protection (RDP)** at the silicon level. This is worth explaining plainly, because the working group will otherwise talk past each other about what "locking the board" actually forecloses — and, as important, what it does not: **RDP protects the firmware image inside the microcontroller. It does nothing for the hardware around it.** The PCB, the schematic, and the bill of materials are exactly as reverse-engineerable on an RDP2-locked board as on a fully open one (§4) — RDP is a firmware secrecy tool, not a hardware one, and closed-hardware manufacturers should not expect it to be read as more than that.

- **RDP Level 0 — unprotected.** Full debug and flash-read access. Anyone with a $10 programmer can dump the firmware. This is effectively the same as fully open source at the object-code level (though not necessarily at the source/readable-code level) — it offers no commercial IP protection at all, but it is maximally scrutineerable and trivially cloneable.
- **RDP Level 1 — reversible protection.** Flash read-out via the debug port is blocked while the protection is active. It *can* be reverted to Level 0, but doing so triggers a full mass-erase of flash as a side effect — so an attacker (or a scrutineer) can regain debug access, but only by destroying the very firmware they wanted to inspect. This is the pragmatic middle ground: it stops casual cloning and IP theft, while still leaving a manufacturer or a court-ordered inspection a path to *prove* what's on a specific unit, at the cost of that unit's firmware. It is compatible with an escrow-based dispute process (see below).
- **RDP Level 2 — permanent, irreversible lock.** Debug access is disabled forever; there is no path back to Level 0 or Level 1, on that chip, ever. This gives absolute IP protection but also forecloses *any* future verification of that specific unit's firmware — by the manufacturer, by a scrutineer, by a court, or by the owner if the manufacturer disappears. It also forecloses field firmware updates via the debug interface (updates would have to go through a signed bootloader instead, which is its own trust question).

**Implication for this standard:** RDP Level 2 is not compatible with any homologation model that relies on being able to read firmware, ever, under any circumstances — including future dispute resolution. It *is* compatible with the behavioral homologation model in §6, because that model never asks to read firmware in the first place. This is a strong argument for the working group to explicitly adopt behavioral (Kₑ/speed-vs-voltage) homologation as the *only* compliance mechanism, precisely because it is the one mechanism that works identically regardless of what RDP level any given manufacturer chooses. Manufacturers who want RDP2-level IP protection should be free to have it; the standard should simply never depend on that protection being absent.

Two practical safeguards the working group should still consider, independent of RDP level:

- **A witnessed "golden sample" escrow.** At homologation, the manufacturer deposits a unit (and, where the manufacturer is willing, a firmware hash or a signed binary) with BSCRA/ISRA or a neutral third party. This doesn't require breaking anyone's RDP lock — a cryptographic hash of the flash image, generated once by the manufacturer at RDP1 before locking to RDP2, is enough to detect a later swap without ever exposing the code itself.
- **Anti-swap / tamper evidence at the connector or enclosure level**, since a unit that measures correctly on the bench but is swapped for a different one on race day defeats behavioral homologation regardless of firmware protection. This is a mechanical/procedural scrutineering problem, not an electronics one, and should be handled the same way control-part scrutineering already handles it in other classes (sealed units, marked/serialized parts, post-race pull-and-check).

---

## 8. Draft charter and principles (for working-group ratification)

These are proposed for the technical working group to adopt, amend, or reject — they are the "consensus target" this paper is trying to help the community reach, not a fait accompli.

1. **Racing fairness is defined by a measurable, published performance envelope — not by the implementation method used to achieve it.** Open-source and closed-source designs are equally eligible if they meet spec.
2. **BSCRA and ISRA, through a joint technical working group, publish and maintain a Design Spec** describing the performance envelope, mechanical/electrical interface, and required safety behavior for eComs in each affected class. The bodies do not mandate a single reference implementation or vendor.
3. **Any developer or manufacturer — hobbyist, open-source project, or commercial company — may submit an implementation for homologation** against the current Design Spec, on equal terms.
4. **Homologation is behavioral and external**, using bench and trackside measurement (per §6, building on the Kₑ tachometer method already proposed in this repository), and never requires disclosure of source code, schematics, or firmware images. RDP-locked (including RDP Level 2) hardware is fully eligible.
5. **Trackside scrutineering must be fast, non-destructive, and require no special access** — target under [X] minutes per car, using equipment any club-level scrutineer can operate without vendor cooperation.
6. **A public homologation register** (make, model, firmware version/hash, measured envelope, homologation date) is maintained and published by BSCRA/ISRA.
7. **Any firmware or hardware revision that changes measured behavior invalidates homologation** until re-tested; field-updatable units must either disable updates during competition or re-submit each version.
8. **Open-source and DIY-built eComs are explicitly protected as a legitimate category**, not merely tolerated — the spec must not be written in a way (e.g., requiring proprietary certification tooling, or per-unit fees unaffordable to a hobbyist) that structurally excludes non-commercial builders.
9. **Commercial manufacturers' IP rights are equally protected** — the standard is a performance spec, not a mechanism for compelling disclosure, and it must not be used to force open designs into commercial products in violation of existing licenses (e.g., Remora's modified CC terms) or to force closed manufacturers to open their firmware.
10. **Spec changes go through a transparent, published process** (an RFC-style proposal, comment period, and working-group vote) with standing representation from BSCRA, ISRA, at least one open-source maintainer, and at least one commercial manufacturer, so no single interest group can unilaterally rewrite the rules.
11. **Disputes over a specific unit's compliance** are resolved through the escrow/golden-sample mechanism (§7), not through demands for public source disclosure.

---

## 9. Objectives

**Near term (next 6–12 months):**
- Convene the joint BSCRA/ISRA technical working group with representation from SBM, Remora, Latslot, BSL, and active club scrutineers.
- Ratify a first-draft Design Spec for classes where the vendor question is still open (ISRA Eurosport categories), and a corresponding verification/scrutineering spec for classes like BSCRA Intro 32 where a single-vendor pairing (MidAmerica 3000Kv + Trixie 2) is already mandated.
- Build and field-trial the Kₑ tachometer scrutineering device described in this repository's `scrutineering.pdf`, and publish tolerance bands.
- Publish that device as an open reference design (schematic + Gerbers) so any manufacturer can build and sell a motor/eCom tester under an open licensing model — ensuring scrutineering equipment doesn't bottleneck on one vendor, and giving builders a concrete, ready-made commercial opportunity.

**Medium term (12–24 months):**
- Stand up the public homologation register and run the first round of manufacturer submissions.
- Pilot the golden-sample escrow process with at least one closed-firmware manufacturer.
- Publish the RFC process for spec changes and hold the first open comment period.

**Long term:**
- Extend the model to additional classes as brushless adoption grows.
- Periodically review whether the spec itself needs to evolve as motor and eCom technology moves (e.g., sensored vs. sensorless commutation, field-oriented control) — the spec should describe outcomes, not freeze today's implementation choices in place.

---

## 10. Open questions for the community

This paper deliberately does not answer these — they are exactly the points on which the working group needs to build consensus:

- Should behavioral tolerance bands be identical across all manufacturers, or class-specific and possibly manufacturer-declared (self-certified envelope, spot-checked)?
- What is the acceptable trackside scrutineering time budget per car, and does the Kₑ tachometer approach actually hit it at real event scale?
- Should the governing bodies formally recognize "licensed" open-hardware builders (e.g., Remora 2 licensees) differently from unlicensed clones of the same open design — and if so, how is that enforced without becoming a de facto single-vendor mandate through the back door?
- Who bears the cost of homologation testing — the manufacturer, the governing body, or race entry fees — and does that cost structurally disadvantage small/hobbyist builders relative to funded commercial ones?
- Is a golden-sample escrow acceptable to closed manufacturers in practice, or does it functionally require RDP1 (reversible) rather than RDP2 (irreversible) at time of submission — and is the working group willing to require that as a homologation precondition?
- How should the standard handle a manufacturer going out of business mid-season — does homologation survive the company, or does the sport need a sunset/grandfathering rule?
- Should the spec explicitly reserve room for firmware experimentation — e.g. a research or exhibition class outside the homologation register — so that early-stage innovation like Infinity ESC isn't discouraged by prematurely freezing what a "compliant" eCom looks like?
- Since two identical-looking locked units may run different firmware, should homologation and retail sale require a human-readable, tamper-resistant version marking on every unit (matching the register), even though the code itself can't be disclosed?
- Now that BSL's own testing has found a measurable timing-advance and RPM-ceiling gap between its two approved eComs, what happens next: retroactive re-homologation against a shared tolerance band, an immediate adjustment mandate on one board, or withdrawal of one part from the approved list until parity is demonstrated?
- Should homologation cover only "as shipped" factory configuration, or must it also bound what a vendor's own programmer/tuning tool is permitted to adjust in the field — given the AART/Latslot precedent shows advance and braking are field-configurable across at least two vendors' ecosystems, and a compliant unit can be retuned out of spec after homologation with no firmware change at all?
- Is a negotiated, partial-disclosure compromise like AART and Latslot's (open-source the patch, withhold the underlying protocol spec) a model the working group should formalize — e.g. a standing NDA-plus-open-release template — for future cross-vendor interoperability work, or a one-off that shouldn't be relied on?

---

## Sources and further reading

- Bob Budge, Richard Mack, and Peter Smits' original brushless slot car development (2021) — [Brushless motored slot cars, slotcarracing.org.uk](https://www.slotcarracing.org.uk/tech/brushless/)
- BSCRA motor and eCom rules — [Motors Used in BSCRA Racing](https://slotcarracing.org.uk/motor/), [Brushless Motors](https://www.slotcarracing.org.uk/motor/brushless2.html)
- ISRA Eurosport brushless adoption — community reporting via [SlotForum: Brushless Slot Car Racing Association](https://www.slotforum.com/threads/brushless-slot-car-racing-association.207101/)
- Remora ESC/eCom project and ESCape32 firmware — [Remora ESCs — Slotblog](https://slotblog.net/topic/104886-remora-escs/), [KC Racing Brushless Mafia Remora background](https://www.kcracing.net/gallery/BM%20Remora.pdf)
- Jan Thoen's Infinity ESC firmware, built on the open Remora hardware base — [gitlab.com/slotracing/infinity-racing/esc](https://gitlab.com/slotracing/infinity-racing/esc)
- This repository's own scrutineering proposal — `scrutineering.pdf` (Kₑ/Kᵥ tachometer method)

*This document lives in the `standardizing-ecoms` repository as a working draft. Comment, fork, and propose edits through whatever process the working group adopts — the point of writing it down is to argue about a fixed target instead of past each other.*
