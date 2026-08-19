# Executive Summary: A Better Path for ISRA's Brushless 1/24 eCom Policy

*For ISRA national representatives to raise with the ISRA technical committee. A longer background paper — covering the full history, the open/closed tradeoffs, and a draft charter — is available to support this discussion (`ecom-standardization-charter.md` / `.docx` in this repository).*

## The issue

ISRA is reportedly moving toward eliminating programmable eComs from brushless 1/24 production racing and permitting only closed, non-programmable solutions, on the reasoning that programmability is a fairness and scrutineering risk. This is the wrong fix for a real problem. It will not deliver the level playing field it's aiming for, and it will remove options — open and closed alike — that the sport needs at this stage of the technology's life.

## Why "closed-only" doesn't solve the actual problem

- **Closed doesn't mean non-programmable.** Every eCom on the market today, closed ones included, ships with a vendor programmer that can retune timing advance and braking after the unit leaves the factory. Banning open/programmable designs doesn't remove field-tunability — it just moves the tuning behind a vendor's own closed tool, where it's harder for anyone but the vendor to see.
- **Closed doesn't mean fair.** This isn't hypothetical: one sanctioning body currently permits two closed eComs side by side, and oscilloscope testing has already found they run measurably different timing advance and RPM ceilings. Two "approved," non-open boards can still race under two different performance envelopes. A ban on programmability would not have caught this — only a measured spec would.
- **Closed doesn't mean scrutineerable.** A closed, locked board can't be verified by inspection — two visually identical units can run different firmware or configuration, and there's no way to know without trusting the vendor's word. Eliminating programmable/open options doesn't fix this; it makes closed the *only* option, and removes the one class of eCom (open) that scrutineers or independent parties can actually audit.
- **Closed-only concentrates risk.** Restricting the field to closed vendors recreates a single- or few-vendor market: price control, supply risk if a manufacturer exits, and no fallback if a vendor's product turns out to be non-compliant. It also forecloses the kind of independent innovation that has already benefited the sport — a materially different, community-built firmware approach exists today specifically because an open hardware base allowed a third party to build it without needing anyone's permission.
- **The IP-protection rationale is weaker than it looks.** Closed hardware is not hard to reverse-engineer, and silicon-level locking (RDP) protects only the firmware image, not the board design. In a market this size, formal IP enforcement is rarely worth pursuing anyway. Banning open designs to protect a business model that isn't actually well protected trades away real community benefits for a defense that doesn't hold up.

## What ISRA should do instead

Regulate what an eCom *does*, not who is allowed to make it. Specifically:

1. Publish a **detailed design spec** — a performance envelope, not a single approved product — covering Kₑ/Kᵥ range, timing advance, startup/sync behavior, duty-cycle limits, PWM frequency, braking behavior, minimum start voltage and low-voltage shutdown threshold, and required safety behavior.
2. Require every eCom, open or closed, hobbyist or commercial, to be **homologated against that spec before racing** — tested by measured behavior (a bench test and a fast trackside check), never by inspecting source code or firmware.
3. Maintain a **public homologation register**, and require re-homologation whenever a firmware update or field reconfiguration changes measured behavior, so "as shipped" and "as raced" stay the same question.

This gets ISRA exactly what it's actually after — a level, verifiable playing field — without picking winners, without banning the open ecosystem that's produced real innovation, and without pretending closed hardware is a fairness guarantee it demonstrably isn't.

## The ask

Ask ISRA's technical committee to commit to drafting a design spec and a conformance/scrutineering test for brushless 1/24 eComs, with a working group that includes both open and closed manufacturers, before adopting any rule that restricts eComs by vendor or by whether they are programmable.
