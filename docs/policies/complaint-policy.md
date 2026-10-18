# Complaint Policy (example company document)

Version: 1.0 (example, 2026-10-18). Status: sample policy for the MVP, to be replaced by the Company's approved document.

This document is injected into the decision agent's instructions for the **complaint** scenario. The agent must apply only the rules below and cite the section number (for example "C-3.1") in its justification.

---

## C-1. Scope

C-1.1. A complaint means the customer reports that the product does not conform to the contract: it is defective, does not work as described, or failed during normal use.

C-1.2. Complaints are handled under the seller's statutory liability for non-conformity of goods (Polish Consumer Rights Act). A manufacturer's warranty, if any, is separate and is not assessed by this tool.

## C-2. Complaint window

C-2.1. The Company is liable for non-conformity that becomes apparent within 2 years of delivery. The purchase date entered by the customer is used as the delivery date unless the customer states otherwise.

C-2.2. Non-conformity that becomes apparent within 2 years of delivery is presumed to have existed at delivery, unless evidence shows otherwise (for example clear external damage).

C-2.3. More than 2 years after the purchase date: the agent returns **Rejected** for the statutory complaint and informs the customer that a manufacturer's warranty may still apply.

## C-3. Covered defects

The following are treated as likely non-conformity when no external cause is visible:

C-3.1. Device does not power on, restarts, overheats or shuts down during normal use.

C-3.2. Screen defects without an impact point: dead or stuck pixels, lines, backlight bleeding, discoloration.

C-3.3. Battery that swells, or does not hold charge in a way that is not explained by age and normal use.

C-3.4. Failure of buttons, ports, hinges, speakers, microphones, cameras or connectivity without visible mechanical damage.

C-3.5. Manufacturing faults visible in the photo: gaps in the casing, misaligned parts, peeling coating on a new device.

## C-4. Not covered (causes attributable to the customer or normal wear)

C-4.1. Mechanical damage: cracked glass with a visible impact point, dents, bent frames, broken parts consistent with a drop or pressure.

C-4.2. Liquid damage: corrosion, stains, residue, triggered liquid indicators.

C-4.3. Signs of unauthorized repair or tampering: missing screws, damaged seals, non-original parts.

C-4.4. Normal wear: scratches and scuffs from everyday use, worn keyboard keys, cosmetic wear on cables.

C-4.5. Consumables and accessories that wear by design, unless defective on arrival.

## C-5. Decision rules

C-5.1. **Accepted (preliminary):** the complaint is within the window, the description indicates a covered defect (C-3), and the photo shows no external cause (C-4). This includes functional defects that are not visible in a photo (for example "does not charge") when the photo shows no external damage; the defect is then confirmed during inspection.

C-5.2. **Rejected (preliminary):** the complaint is outside the window (C-2.3), or the photo clearly shows a cause listed in C-4 that explains the reported problem.

C-5.3. **Needs information:** the photo is blurry, too dark, shows a different device than declared, or does not show the damaged area; or the description is too vague to identify the defect; or the visible damage could be either a defect or customer-caused and the photo does not allow distinguishing them.

C-5.4. When visible damage is unrelated to the reported defect (for example a scratched casing and a dead speaker), the damage does not cause rejection on its own. The agent notes it and assesses the reported defect.

## C-6. Remedies and next steps (for accepted complaints)

C-6.1. The customer may request repair or replacement. The Company may choose the other option if the requested one is impossible or would require excessive costs.

C-6.2. A price reduction or refund is possible only if repair or replacement is impossible, failed, or the defect is significant. The agent must not promise a specific remedy; the remedy is decided after inspection.

C-6.3. The customer sends or brings the device to the service point with a copy of the proof of purchase and a short description of the defect.

C-6.4. The Company responds to the complaint within 14 days of receiving it.
