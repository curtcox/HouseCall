# Use Cases: Appropriate Followup as a Service

The service accepts situations to monitor, actively monitors them over time, and escalates or de-escalates as needed. It gathers, stores, and distributes information for the benefit of its users. Determining when and how to do that in the most effective and appropriate way is the hard problem to solve. It continues to work proactively over time acting to achieve the best outcome.

---

## Use Case 1: Child's Nighttime Medical Concern

### Setup

- **Who:** A parent and their child.
- **What:** The child wakes up with an unusual sensation in her arm and cannot sleep.
- **When:** Middle of the night, around 2:00 AM.
- **Where:** At home.
- **Why:** The parent needs to determine whether this requires emergency care, can wait until morning, or is nothing to worry about.
- **How:** The parent starts the service and receives a phone call. The voice interviews both the parent and the child, gathering symptoms, history, and context.

### Timeline

**2:00 AM - Initial contact.**
The service calls the parent. It speaks with the parent and, with permission, speaks to the child. It asks about the sensation — tingling, pain, numbness, duration, any recent injuries or activities. It asks about the child's medical history. It determines that the symptoms do not indicate an emergency.

**2:15 AM - Initial assessment delivered.**
The service tells the parent there is no indication of an emergency. It recommends comfort measures and sleep. It schedules a followup call for 7:00 AM.

**7:00 AM - Morning followup.**
The service calls back. It asks about the child's current state. Did she sleep? Is the sensation still present? Has it changed?

**Continuing followup.**
The service periodically calls to ask followup questions, adjusting frequency based on whether symptoms are improving, stable, or worsening.

**Resolution.**
The case closes when the child is fully recovered and no further monitoring is needed.

### Variant A: Symptoms resolve by morning

**7:00 AM** - Child slept fine and sensation is gone. Service confirms no further action needed. Case closed.

### Variant B: Symptoms persist but are stable

**7:00 AM** - Sensation still present but unchanged. Service recommends scheduling a pediatrician visit and offers to help find an available appointment. Follows up after the appointment to capture the doctor's findings.

**10:00 AM** - Service calls to confirm the appointment was made. It records the appointment time and sets a followup for after.

**3:00 PM** - Service calls after the appointment window. Parent reports the doctor said it was a minor nerve issue. Service records this and schedules a 48-hour followup to confirm resolution.

**Two days later** - Final followup. Child is fine. Case closed.

### Variant C: Symptoms worsen overnight

**4:30 AM** - Parent calls the service back. The child's arm now has visible swelling. The service immediately escalates. It advises going to the emergency room. It offers to notify the child's pediatrician. It provides the nearest ER address and current wait time if available.

**5:00 AM** - Service follows up to confirm the family has arrived at the ER or is en route.

**8:00 AM** - Service calls for a post-ER update. Captures diagnosis and treatment plan.

**Ongoing** - Service follows up on recovery per the treatment plan timeline. Case closes when the child is recovered.

### Variant D: Parent is unsure and anxious

**2:15 AM** - The service's initial assessment is no emergency, but the parent is clearly anxious and wants more certainty. The service offers to connect the parent with a nurse advice line or telehealth provider. It stays on the line to help coordinate the handoff.

**2:45 AM** - Telehealth consultation confirms no emergency. Service records this and proceeds with standard morning followup.

---

## Use Case 2: Data Center Installation Behind Schedule

### Setup

- **Who:** An on-site team lead, a boss at the central office, and the on-site installation crew.
- **What:** A data center installation has passed its stated completion date. Management wants around-the-clock work with status updates every 2 hours.
- **When:** The deadline has passed. Urgency is immediate.
- **Where:** On-site at the data center, with remote management at central office.
- **Why:** The project is behind schedule and management needs visibility and control over the recovery effort.
- **How:** The team lead starts the service and receives a phone call. The service interviews the team lead and the boss, then begins coordinating shifts, status calls, and reporting.

### Timeline

**Day 1, 10:00 AM - Initial contact.**
The service calls the team lead. It gathers: what work remains, how many people are available, what the blockers are, and what the boss's expectations are. It then calls the boss to confirm expectations and reporting format.

**Day 1, 11:00 AM - Coordination begins.**
The service contacts available crew members to arrange a shift schedule for around-the-clock coverage. It creates an online status dashboard accessible to the boss and team.

**Day 1, 12:00 PM - First status meeting.**
The service sets up and attends a status call. It captures what was accomplished, what's next, and any new blockers. It updates the dashboard.

**Every 2 hours thereafter** - The service repeats the status cycle. It attends or facilitates each call, records status, updates the dashboard, and flags any issues that need the boss's attention.

**Ongoing** - The service adjusts shift assignments as people become fatigued or unavailable. It proactively identifies when supplies or resources are running low and alerts the appropriate person.

**Resolution.**
The installation is complete. The service conducts a final status update, archives the dashboard, and closes the case.

### Variant A: Smooth recovery

The around-the-clock shifts go as planned. Progress is steady. The service reports good news at each check-in. The installation completes within 48 hours. Case closed.

### Variant B: Key person unavailable

**Day 1, 3:00 PM** - A critical technician calls in sick. The service identifies the gap in the shift schedule, contacts available replacements, and rearranges the schedule. It notifies the team lead and boss of the change and any impact on the timeline.

### Variant C: Scope change mid-recovery

**Day 2, 8:00 AM** - The boss decides to add additional equipment to the installation while the team is already on-site. The service captures the new requirements, assesses the impact on the existing schedule, updates the dashboard, and adjusts the shift plan and resource needs accordingly.

### Variant D: Escalating frustration between team and management

**Day 2, 2:00 PM** - Status calls are becoming tense. The boss is expressing frustration with pace. The team feels micromanaged. The service recognizes the dynamic and adjusts its approach: it provides the boss with more detailed written updates to reduce the need for contentious calls, and it gives the team lead a summary of management's priorities so the team can focus on what matters most.

---

## Use Case 3: Elderly Parent Fall at Home

### Setup

- **Who:** An adult child (who lives out of state) and their elderly parent.
- **What:** The elderly parent fell at home and called their child. The parent says they are fine but sore.
- **When:** Mid-afternoon on a weekday.
- **Where:** The parent lives alone in another state.
- **Why:** The adult child cannot be there in person and needs help monitoring the situation and determining if medical attention is needed.
- **How:** The child starts the service. The service calls the parent directly to assess the situation.

### Timeline

**2:00 PM - Initial contact.**
The service calls the parent. It asks about the fall — how it happened, where they fell, whether they hit their head, current pain level, ability to stand and walk. It asks about medications (blood thinners are a particular concern). It asks if there is anyone nearby who could check in.

**2:20 PM - Assessment delivered.**
The service reports to the adult child: no head injury, no signs of fracture, parent is ambulatory but sore. Service recommends monitoring for 24 hours, especially for delayed symptoms like confusion, severe bruising, or inability to bear weight.

**6:00 PM - Evening followup.**
The service calls the parent to check in. How is the pain? Any new symptoms? Did they eat dinner? Can they move around safely?

**Next morning, 8:00 AM** - Morning followup. How did they sleep? Any new bruising? Are they steady on their feet?

**Ongoing** - Followup calls continue, decreasing in frequency as the parent improves.

**Resolution.**
The parent is back to baseline. Case closed.

### Variant A: Parent minimizes symptoms

**6:00 PM** - The parent says they are fine, but the service detects inconsistencies — slower speech, mentions of dizziness when pressed. The service escalates to the adult child and recommends an in-person check. It offers to contact a local neighbor, friend, or welfare check service.

### Variant B: Delayed serious symptoms

**Next morning** - The parent reports significant bruising and cannot put weight on one leg. The service escalates: it recommends urgent medical evaluation, offers to arrange transportation (ride service, ambulance if necessary), and notifies the adult child. It follows up through the medical visit and captures the outcome.

### Variant C: Recurring falls

During followup, the parent mentions this is the third fall in two months. The service flags this pattern to the adult child. It recommends a fall risk assessment and offers information about home safety modifications, physical therapy, and medical alert devices. It follows up to ensure these conversations happen.

### Variant D: Parent refuses help

**2:00 PM** - The parent insists they are fine and does not want to be bothered. The service respects the parent's autonomy but ensures the adult child is informed of the refusal. It offers to check in just once more the next day. It provides guidance to the adult child on how to navigate the situation, including when to override the parent's wishes for safety reasons.

---

## Use Case 4: Home Water Leak Emergency

### Setup

- **Who:** A homeowner.
- **What:** The homeowner discovers water leaking from a ceiling, source unknown.
- **When:** Saturday evening.
- **Where:** At home.
- **Why:** The homeowner needs to stop the damage, find the source, and get it repaired — but it's the weekend and they don't know a plumber.
- **How:** The homeowner starts the service and receives a phone call.

### Timeline

**6:00 PM - Initial contact.**
The service interviews the homeowner. Where is the leak? How fast? Is it near electrical fixtures? What's above the ceiling — another bathroom, the roof? Has anything changed recently (rain, new appliance)? The service provides immediate guidance: shut off water to the suspected source, place containers to catch water, move valuables.

**6:15 PM - Immediate triage delivered.**
The service determines this is not a safety emergency (no electrical risk) but requires prompt attention. It begins contacting emergency plumbers in the area to find weekend availability.

**6:30 PM - Plumber arranged.**
The service finds an available plumber, confirms the cost estimate and arrival window with the homeowner, and provides the plumber's contact information. It sets a followup for after the plumber's expected arrival.

**8:00 PM - Post-visit followup.**
The service calls to confirm the plumber arrived and what was found. It captures the diagnosis, repair status, and any further work needed.

**Ongoing** - If the repair requires follow-on work (drywall repair, mold inspection), the service tracks those items and follows up until everything is resolved.

**Resolution.**
All repairs complete. Case closed.

### Variant A: Quick fix

The plumber arrives, finds a loose fitting, tightens it, done. Service confirms and closes the case the next day after verifying no further leaking.

### Variant B: Serious pipe burst

**6:30 PM** - No plumber is available for 4 hours. Meanwhile, the leak is worsening. The service advises shutting off the main water supply and walks the homeowner through locating the shutoff valve. It escalates its search for an available plumber and contacts the homeowner's insurance company to begin a claim.

**10:00 PM** - Plumber arrives. Major pipe replacement needed. Service follows up over the coming days to track repair completion, insurance adjuster visit, and restoration work.

### Variant C: Landlord involvement

The homeowner is actually a renter. The service contacts the landlord or property management company on the renter's behalf, documents the damage with timestamps and descriptions, and follows up to ensure the landlord takes action. It tracks the repair and follows up with the renter on livability.

### Variant D: Mold discovered during repair

During the pipe repair, mold is found behind the wall. The service escalates by recommending a mold inspection, helps find a remediation company, and tracks the expanded scope of work. It adjusts its followup timeline to account for the longer remediation process.

---

## Use Case 5: International Travel Disruption

### Setup

- **Who:** A business traveler.
- **What:** The traveler's connecting flight is canceled while they are in a foreign airport where they do not speak the language.
- **When:** Late evening local time.
- **Where:** A foreign airport.
- **Why:** The traveler needs to get rebooked, find accommodation if needed, and communicate the disruption to people expecting them at the destination.
- **How:** The traveler starts the service and receives a phone call.

### Timeline

**10:00 PM local - Initial contact.**
The service gathers information: what airline, what route, when is the final destination needed, does the traveler have hotel reservations at the destination, who needs to be notified (employer, client, family). It checks alternative flights and rebooking options.

**10:15 PM - Immediate actions.**
The service contacts the airline's rebooking line on behalf of the traveler (or guides them through it). If no flights are available until morning, it finds nearby airport hotels with availability and helps book one. It notifies the traveler's contacts about the delay.

**Next morning, 6:00 AM** - Wake-up followup. The service confirms the new flight details, reminds the traveler of the gate and time, and verifies the downstream arrangements (hotel at destination, meeting times) have been adjusted.

**Ongoing** - The service monitors the replacement flight status and alerts the traveler to any further changes.

**Resolution.**
The traveler arrives at their destination. Downstream meetings or commitments have been adjusted. Case closed.

### Variant A: Smooth rebooking

An alternative flight is available in 2 hours. The service rebooks, notifies contacts of the slight delay, and monitors until arrival. Case closed on arrival.

### Variant B: Multi-day delay

A weather event has grounded all flights for 48 hours. The service books a hotel for two nights, rearranges the traveler's entire destination schedule, works with the employer to determine whether the trip should be canceled or delayed, and monitors weather updates. It proactively rebooks as soon as flights resume.

### Variant C: Lost luggage compounds the problem

The traveler's luggage did not make it to the connecting airport. The service files a lost luggage claim with the airline, arranges for delivery to the traveler's next location, and helps the traveler find essential items (medication, clothing) in the meantime.

### Variant D: Medical issue during layover

The traveler develops food poisoning at the airport. The service locates airport medical services, helps the traveler access care, determines whether the traveler is fit to fly, and rearranges travel plans if needed. It notifies the traveler's emergency contacts.

---

## Use Case 6: Contractor Home Renovation Oversight

### Setup

- **Who:** A homeowner who has hired a contractor for a kitchen renovation.
- **What:** The renovation is underway but the homeowner works full time and cannot supervise daily.
- **When:** A multi-week renovation project.
- **Where:** The homeowner's residence.
- **Why:** The homeowner wants to stay informed of progress, catch problems early, and ensure the contractor stays on schedule and on budget.
- **How:** The homeowner starts the service at the beginning of the project.

### Timeline

**Day 1 - Project kickoff.**
The service calls the homeowner to gather project details: scope of work, budget, timeline, contractor contact information, key milestones, and the homeowner's communication preferences. It then introduces itself to the contractor (with the homeowner's authorization) as a project coordination assistant.

**Daily** - The service contacts the contractor at the end of each work day to gather a status update: what was done today, any issues encountered, any deviations from plan. It summarizes this for the homeowner.

**Weekly** - The service provides the homeowner with a weekly summary: percent complete, budget spent vs. remaining, upcoming milestones, and any concerns.

**As needed** - When decisions are required (material choices, change orders), the service coordinates between the homeowner and contractor to get timely answers and prevent work stoppages.

**Resolution.**
Renovation complete, final walkthrough done, punch list items resolved. Case closed.

### Variant A: On time and on budget

Everything goes smoothly. The service's role is primarily informational — keeping the homeowner updated. Case closes at project completion.

### Variant B: Budget overrun detected early

**Week 2** - The service notices that spending is trending ahead of the budget. It flags this to the homeowner with specifics: what cost more than expected and why. It facilitates a conversation between the homeowner and contractor about options to stay on budget.

### Variant C: Contractor goes silent

**Day 8** - The contractor does not respond to the daily check-in call. The service tries again the next morning. After 48 hours of no contact, it escalates to the homeowner and recommends next steps: contacting the contractor directly, reviewing the contract terms, and beginning to identify backup contractors.

### Variant D: Quality concern

**Week 3** - The homeowner visits the site on a weekend and notices the tile work looks crooked. They report this to the service. The service documents the concern, raises it with the contractor at the next check-in, and follows up to ensure it is corrected before work proceeds further.

---

## Use Case 7: Chronic Condition Medication Change

### Setup

- **Who:** A patient who has just been prescribed a new medication for a chronic condition.
- **What:** The patient's doctor has changed their blood pressure medication.
- **When:** Ongoing — the new medication needs to be monitored for effectiveness and side effects over weeks.
- **Where:** The patient's daily life.
- **Why:** Medication changes for chronic conditions require monitoring, but doctor visits are weeks apart. The patient needs support in the interim.
- **How:** The patient starts the service after leaving the doctor's office.

### Timeline

**Day 1 - Initial setup.**
The service calls the patient to gather information: what medication, what dose, when to take it, what side effects to watch for (per the doctor's instructions), and what the patient's baseline symptoms are.

**Daily for the first week** - The service calls at the patient's preferred time to check in: did they take the medication, any side effects noticed, current blood pressure reading (if they have a home monitor), any concerns.

**Week 2 onward** - If all is stable, frequency drops to every other day, then twice a week.

**Before the follow-up appointment** - The service compiles a summary of the patient's daily reports and sends it to the patient to share with their doctor.

**Resolution.**
The doctor confirms the medication is working well and no further close monitoring is needed. Case closed.

### Variant A: Medication works well

No significant side effects. Blood pressure improves. The service provides a positive summary for the doctor visit. Case closed.

### Variant B: Side effects emerge

**Day 4** - The patient reports persistent dizziness. The service asks detailed questions to characterize the severity and timing. It recommends the patient contact their doctor and offers to help schedule an urgent appointment. It increases check-in frequency back to daily.

**Day 6** - Doctor adjusts the dose. The service resets its monitoring baseline and begins tracking the new dose.

### Variant C: Patient forgets to take medication

**Day 3** - The patient admits they forgot the medication yesterday. The service asks if this is a recurring issue and offers strategies: setting phone alarms, keeping medication in a visible place, using a pill organizer. It notes the missed dose for the doctor summary.

### Variant D: Emergency symptom

**Day 5** - The patient reports chest pain. The service immediately advises calling 911 or going to the nearest emergency room. It does not attempt to diagnose. It follows up after the emergency to learn the outcome and adjusts its monitoring accordingly.

---

## Use Case 8: Job Candidate Pipeline Management

### Setup

- **Who:** A small business owner hiring for a critical role.
- **What:** The owner is interviewing multiple candidates over several weeks and needs to keep the process organized and responsive.
- **When:** A multi-week hiring process.
- **Where:** Coordinated remotely — candidates are in different locations.
- **Why:** The owner is running the business and doesn't have an HR department. Slow responses lose good candidates.
- **How:** The owner starts the service after posting the job listing.

### Timeline

**Day 1 - Setup.**
The service gathers: the job description, must-have vs. nice-to-have qualifications, the interview process (phone screen, technical interview, final interview), scheduling constraints, and how the owner wants to evaluate candidates.

**As applications arrive** - The service reviews applications against the criteria, presents a summary of each candidate to the owner, and recommends which to advance. The owner makes the decision; the service executes it.

**Scheduling** - The service contacts candidates to schedule phone screens, coordinates calendar availability, sends confirmations and reminders.

**After each interview** - The service calls the owner to capture feedback while it's fresh. It maintains a comparison matrix of all candidates.

**Ongoing** - The service sends timely updates to candidates who are waiting, keeping them engaged. It flags when a top candidate might be losing interest or has a competing offer.

**Resolution.**
An offer is extended, accepted, and a start date is set. The service notifies the other candidates. Case closed.

### Variant A: Clear top candidate

One candidate stands out early. The service accelerates the process for that candidate while maintaining the pipeline as backup. Offer extended quickly. Case closed.

### Variant B: Candidate drops out

**Week 2** - The top candidate accepts another offer. The service immediately surfaces the next best candidates and adjusts the timeline. It recommends whether to re-post the listing or proceed with the current pool.

### Variant C: Owner can't decide

**Week 3** - Two finalists are very close. The service organizes a structured comparison, suggests a final evaluation step (reference checks, work sample), and facilitates the decision-making process without rushing the owner.

### Variant D: Candidate has concerns

**Week 2** - During an interview, a candidate raises concerns about the role. The service captures these, helps the owner address them (adjusting the offer, clarifying expectations), and follows up with the candidate to see if the concerns are resolved.

---

## Use Case 9: Community Event Crisis Response

### Setup

- **Who:** A volunteer coordinator organizing a community fundraising event.
- **What:** The outdoor event is tomorrow and severe weather is forecast.
- **When:** The evening before the event, with the event scheduled for the next morning.
- **Where:** A public park. A backup indoor venue has been tentatively identified but not confirmed.
- **Why:** Hundreds of people are expected. Vendors, volunteers, and attendees need to be coordinated quickly if plans change.
- **How:** The coordinator starts the service when the weather forecast turns bad.

### Timeline

**7:00 PM evening before - Initial contact.**
The service gathers: event details, number of attendees expected, vendor list, volunteer list, communication channels in use, the backup venue option, and the decision deadline for moving indoors.

**7:30 PM - Action plan.**
The service confirms the backup venue's availability and capacity. It drafts communication messages for three scenarios: event as planned, event moved indoors, event postponed. It presents these to the coordinator for approval.

**9:00 PM - Weather check.**
The service checks the latest forecast and reports to the coordinator. A decision is needed by 6:00 AM.

**6:00 AM event day - Decision point.**
The service calls the coordinator with the latest forecast. The coordinator decides to move indoors. The service immediately begins executing the communication plan: notifying vendors, volunteers, and attendees through the pre-approved channels.

**7:00 AM - Confirmation sweep.**
The service follows up to confirm key vendors and volunteers have received the message and are adjusting plans.

**During the event** - The service remains available to handle coordination issues as they arise.

**Post-event** - The service gathers attendance numbers and feedback, and sends a thank-you summary to volunteers and vendors.

**Resolution.**
Post-event wrap-up complete. Case closed.

### Variant A: Weather clears

**6:00 AM** - Forecast improves. Event proceeds as planned. The service stands down but remains available during the event for any issues.

### Variant B: Backup venue falls through

**8:00 PM** - The backup venue is not available. The service immediately begins searching for alternatives, contacting community centers, schools, and churches. It presents options to the coordinator with capacity and logistics details.

### Variant C: Event postponed

**6:00 AM** - Weather is dangerous. The coordinator decides to postpone rather than move indoors. The service executes the postponement communication plan, helps select a new date, and begins the coordination cycle again for the rescheduled event.

### Variant D: Partial execution

**6:00 AM** - Weather is marginal. The coordinator moves some activities indoors but keeps others outside under tents. The service coordinates the split plan, ensuring each vendor and volunteer knows their updated location.

---

## Use Case 10: Vehicle Breakdown and Repair Coordination

### Setup

- **Who:** A driver.
- **What:** The driver's car breaks down on the side of a highway.
- **When:** Late afternoon on a weekday, with the driver needing to pick up children from daycare by 6:00 PM.
- **Why:** The driver needs to solve two problems simultaneously: the car situation and the child pickup.
- **How:** The driver starts the service from the side of the road.

### Timeline

**4:30 PM - Initial contact.**
The service gathers: location, what happened (car died, no warning lights, won't restart), safety situation (pulled over on shoulder, hazards on), and the time-critical constraint (daycare pickup at 6:00 PM). The service identifies the two parallel workstreams.

**4:35 PM - Parallel actions begin.**
Workstream 1 (car): The service contacts roadside assistance or arranges a tow truck. It identifies the nearest repair shop and checks availability.
Workstream 2 (children): The service contacts the driver's designated backup person (spouse, relative, friend) to arrange the daycare pickup. If the primary backup is unavailable, it tries the secondary contact. It notifies the daycare that pickup arrangements are being made.

**5:00 PM - Status update.**
The service calls the driver: tow truck is en route (ETA 30 minutes), backup person confirmed for daycare pickup. Driver can focus on the car situation.

**5:30 PM - Tow arrives.**
The service confirms the car is being towed to the repair shop. It arranges a ride for the driver from the repair shop to home (or to the daycare, or wherever they need to be next).

**Next day** - The service follows up with the repair shop for a diagnosis. It relays the estimate and timeline to the driver. If the repair will take multiple days, it helps the driver explore rental car options.

**Resolution.**
Car repaired and picked up. Case closed.

### Variant A: Quick roadside fix

The tow truck driver identifies a dead battery and jump-starts the car. The service arranges for a battery replacement at a nearby shop and follows up to confirm it's done. Daycare backup is stood down. Case closed quickly.

### Variant B: No backup available for daycare

**4:40 PM** - Neither the primary nor secondary backup can do the pickup. The service helps the driver contact the daycare to arrange a late pickup, explores whether a trusted friend or neighbor could help, and in the worst case helps the driver arrange a ride from the breakdown site to the daycare directly, dealing with the car situation afterward.

### Variant C: Major repair needed

The repair shop reports the transmission needs replacement — several days of work and thousands of dollars. The service helps the driver arrange a rental car, understand warranty coverage, get a second opinion if desired, and track the repair progress over the coming days.

### Variant D: Insurance and claims

The breakdown is caused by a recent accident repair that was done incorrectly. The service helps the driver document the situation, contact the original repair shop, and file an insurance claim if needed. It tracks the back-and-forth until resolution.

---

## Use Case 11: Student Academic Crisis

### Setup

- **Who:** A college student and their parent.
- **What:** The student has failed a midterm exam and is at risk of failing the course, which could affect their financial aid.
- **When:** Mid-semester.
- **Where:** The student is at college; the parent is at home.
- **Why:** The student is overwhelmed and doesn't know what options are available. The parent wants to help but doesn't know the system.
- **How:** The parent starts the service after a distressed phone call from the student.

### Timeline

**Day 1 - Initial contact.**
The service calls the parent to understand the situation. It then calls the student (with permission) to get details: which course, what grade, what went wrong, what resources have they tried, what are the academic policies (withdrawal deadlines, grade replacement, tutoring).

**Day 1 - Information gathering.**
The service researches the university's academic policies: drop deadlines, incomplete grade options, tutoring center availability, academic advising contacts, and financial aid implications.

**Day 2 - Options presented.**
The service calls the student and parent to present options: meet with the professor to discuss extra credit or grade recovery, engage a tutor, withdraw from the course (with financial aid implications noted), or request an incomplete. It does not make the decision — it presents the information and consequences clearly.

**Day 3 onward** - The service follows up on whichever path the student chooses: did they meet with the professor, did they schedule tutoring, did they contact the registrar?

**Resolution.**
The student has a plan in place and is executing it. The academic situation is stabilized. Case closed.

### Variant A: Professor offers a path to recovery

The professor allows the student to retake the midterm or complete extra work. The service follows up to ensure the student is taking advantage of the opportunity.

### Variant B: Withdrawal is the best option

The drop deadline is approaching. The service helps the student understand the financial and academic implications, confirms the decision with the parent, and follows up to ensure the withdrawal is processed.

### Variant C: Deeper issues surface

During conversations, the service recognizes signs that the student may be dealing with anxiety or depression. It recommends the university counseling center and follows up to ensure the student makes contact.

### Variant D: Financial aid is at risk

The student's GPA is in danger of falling below the financial aid threshold. The service helps the student understand exactly what grades are needed across all courses to maintain aid, and coordinates a plan that accounts for the full picture.

---

## Use Case 12: Pet Emergency After-Hours

### Setup

- **Who:** A pet owner.
- **What:** The owner's dog ingested something potentially toxic (chocolate, medication, etc.).
- **When:** 11:00 PM, after the regular vet is closed.
- **Where:** At home.
- **Why:** The owner doesn't know whether this is a true emergency, and after-hours vet visits are expensive. They need guidance.
- **How:** The owner starts the service.

### Timeline

**11:00 PM - Initial contact.**
The service asks: what was ingested, how much, how long ago, the dog's weight and breed, and current symptoms. It cross-references this with toxicity information.

**11:10 PM - Assessment.**
Based on the amount ingested relative to the dog's weight, the service determines the level of risk. It provides a clear recommendation: go to the emergency vet now, call the ASPCA poison control hotline, or monitor at home.

**If monitoring at home** - The service provides specific symptoms to watch for and schedules check-in calls through the night.

**Next morning** - The service follows up and recommends calling the regular vet for a check-up.

**Resolution.**
The dog is healthy and no further monitoring is needed. Case closed.

### Variant A: Low risk, monitoring is sufficient

The amount of chocolate was small relative to the dog's size. The service monitors overnight. Dog is fine by morning. Regular vet confirms no concern. Case closed.

### Variant B: Emergency vet visit needed

The ingested amount is dangerous. The service identifies the nearest emergency vet clinic, provides the address and phone number, and advises immediate transport. It follows up to capture the treatment and outcome.

### Variant C: Owner can't afford emergency vet

The owner is hesitant to go to the emergency vet due to cost. The service provides information about payment plans offered by emergency clinics, pet credit lines, and charitable assistance programs. It does not override the owner's decision but ensures they understand the risk.

### Variant D: Unknown substance

The owner isn't sure exactly what the dog ate. The service walks through what's accessible in the area where it happened, helps identify the most likely substance, and advises based on the worst-case scenario among the possibilities.

---

## Emerging Patterns

The use cases above reveal consistent patterns that should guide the service's implementation.

### Pattern 1: Intake and Triage

Every case begins with an interview to assess the situation. The service must:
- Gather structured information through natural conversation.
- Assess urgency on a spectrum from "no immediate action needed" to "act now."
- Make a clear initial recommendation.
- Set expectations for what happens next.

### Pattern 2: Parallel Workstreams

Many cases involve multiple things happening at once (car + daycare, installation + reporting, medical + logistics). The service must:
- Identify and manage concurrent concerns.
- Track each workstream independently.
- Coordinate between workstreams when they affect each other.

### Pattern 3: Time-Based Monitoring

Every case involves followup over time, with varying frequencies. The service must:
- Schedule and execute followup contacts.
- Adjust frequency based on the situation's trajectory (improving, stable, worsening).
- Know when to increase or decrease contact frequency.
- Maintain context across all interactions so the user never has to repeat information.

### Pattern 4: Escalation and De-escalation

Situations change, and the service must respond. The service must:
- Recognize when a situation is getting worse and escalate appropriately.
- Recognize when a situation is improving and reduce intensity.
- Have clear criteria for escalation triggers (e.g., symptoms worsening, deadlines approaching, contacts unreachable).
- Escalate to the right person or resource.

### Pattern 5: Multi-Party Coordination

Most cases involve more than one person. The service must:
- Manage communication between multiple parties (patient/parent, team/boss, owner/contractor).
- Keep each party informed at the appropriate level of detail.
- Facilitate conversations and decisions between parties.
- Respect information boundaries (not everything should be shared with everyone).

### Pattern 6: Information Gathering, Storage, and Distribution

The service is fundamentally an information broker. It must:
- Gather information through natural conversation.
- Store structured records of each case with full history.
- Generate summaries and reports appropriate for different audiences.
- Create shared artifacts when needed (dashboards, status pages, comparison matrices).

### Pattern 7: Proactive Action

The service doesn't just monitor — it acts. It must:
- Contact third parties (plumbers, airlines, contractors, clinics) on behalf of the user.
- Schedule and coordinate meetings, shifts, and appointments.
- Arrange logistics (rides, hotels, alternatives).
- Take initiative when waiting would make things worse.

### Pattern 8: Graceful Resolution

Every case must end. The service must:
- Recognize when a case is resolved.
- Confirm resolution with the user.
- Archive case information for future reference.
- Ensure no loose ends remain.

### Pattern 9: Emotional and Social Awareness

The service operates in high-stress situations. It must:
- Recognize when a user is anxious and adjust its communication style.
- Mediate between parties when tensions arise.
- Respect autonomy — present options, don't override decisions.
- Know when to recommend professional support (medical, legal, mental health).

### Pattern 10: Context Persistence and Handoff

The service operates across many interactions over time. It must:
- Maintain complete context across all calls and interactions within a case.
- Never ask the user to repeat information already provided.
- Be able to hand off context if a different channel or responder is needed.
- Keep a complete audit trail of all actions taken and information gathered.

---

## Implementation Implications

These patterns suggest the following core capabilities:

1. **Conversational intake engine** — Structured interviews conducted through natural phone conversation, adapted to the domain of the case.
2. **Case management system** — Persistent records with full history, multi-party visibility, and configurable access.
3. **Scheduling and reminder engine** — Time-based triggers for followups, with dynamic adjustment based on case trajectory.
4. **Escalation framework** — Rules and heuristics for determining when and how to escalate or de-escalate.
5. **Third-party communication** — Ability to contact external parties (businesses, services, individuals) on behalf of the user.
6. **Multi-channel coordination** — Phone calls, status dashboards, written summaries, and potentially messaging.
7. **Domain knowledge base** — Reference information for common scenarios (medical symptom assessment, travel rebooking procedures, insurance claims processes, etc.).
8. **Resolution detection** — Recognizing when a case has reached its natural conclusion.
9. **Audit and reporting** — Complete records of all interactions and actions for accountability and learning.
