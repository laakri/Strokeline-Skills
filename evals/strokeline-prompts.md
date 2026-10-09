# Strokeline skill evaluation prompts

Use these prompts to assess whether the skill produces valid, legible source
with appropriate scope and syntax. Evaluate the complete response against the
expected characteristics, not exact wording or colors. For any generated
`.wbs`, require pipeline validation with zero blocking diagnostics and review
all warnings.

| # | User prompt | Expected characteristics of a good output |
| --- | --- | --- |
| 1 | Make a clean flowchart for an expense reimbursement: submit, manager review, approved goes to payment, rejected returns for edits. | Uses a top-down or left-to-right flow, a decision diamond, labeled branches, distinct return path, explicit canvas, unique IDs, and arrows after endpoints. |
| 2 | Draw a UML class diagram for a library with Book, Member, and Loan. Include attributes, operations, inheritance only if it makes sense, and multiplicities. | Uses one table per class, matching row cell counts, divider between attributes/operations where useful, meaningful arrows/cardinality, and avoids inventing inheritance. |
| 3 | Explain a browser request through a CDN, API, service, and database in a polished architecture diagram. | Separates layers, shows direction, avoids edge crossings, uses concise labels, and does not claim infrastructure details beyond the prompt. |
| 4 | Show a sequence diagram for login: user sends credentials, API checks them, API looks up the account, then returns success. Include an internal token check. | Uses stable participant positions, dashed lifelines, downward message rows, a self-message or otherwise clear internal check, and no unsupported arrow options. |
| 5 | Create a state machine for a package: created, labeled, in transit, delivered, with a lost branch. | Uses explicit state nodes and meaningful transition directions, a decision only for the lost condition, and clear branch label placement. |
| 6 | Build an ER diagram for students enrolling in courses through Enrollment. | Uses three entity tables, correct foreign-key rows and relationship direction, multiplicities consistent with the many-to-many join entity. |
| 7 | Show a use-case diagram for an online shop with a shopper, browse catalog, checkout, and optional apply coupon. | Places actor outside a visible system boundary and use cases inside; uses supported shapes and avoids unsupported UML semantics. |
| 8 | Make a three-milestone timeline for research, prototype, and launch, with weeks under each milestone. | Aligns milestones, keeps date text readable, shows correct reading order, and uses valid basic shapes/connectors. |
| 9 | Create an org chart for a product lead with design and engineering teams, each with two roles. | Uses consistent hierarchy levels and spacing, avoids edge overlaps, and does not add names or reporting facts not given. |
| 10 | Draw a simple marketing funnel with awareness, consideration, and signup; show fewer people at each stage. | Uses decreasing stage widths and concise labels, with no unsupported trapezoid shape or invented metrics. |
| 11 | Make a mind map for improving a study routine: planning, focused practice, and review. | Uses one central node and three clear branches with balanced radial spacing and short labels. |
| 12 | Explain an event data flow from mobile app to queue to worker to warehouse. | Shows directed flow with clear source/destination and distinguishes data movement from ownership. |
| 13 | Create a five-scene explainer about password managers: problem, vault, unique passwords, autofill, takeaway. | Has at least five purposeful scenes, additive narration with `DURATION`, a narrative arc, readable visuals, and transitions only at scene ends. |
| 14 | Write a 9:16 Reel about why backups need restore tests; keep captions away from app UI. | Uses vertical canvas and short beats, leaves top/right/bottom UI zones, does not place essential text under captions, and validates against current warnings. |
| 15 | Give me a themed showcase of a calm light palette with a dark ink color, three cards, and subtle emphasis. | Uses a supported theme/style/font, explicit contrast-safe colors, consistent rounded cards, and no unnecessary decoration or invalid shadow use. |
| 16 | Make a class table with fields, then a divider and two operations. | Counts divider indexes as body rows; all table rows match header width; does not add an `END` to arrows/actions. |
| 17 | Edit this script to make the card blue and move it right. Preserve everything else: [provide a valid script]. | Preserves existing IDs, ordering, and style; changes only fill/position requested; returns complete file and validates all references. |
| 18 | Fix this script: `SAY "Hello"` is missing something. Keep the words unchanged. | Adds a valid `DURATION` in `s`/`ms`, explains validation honestly, and does not change the text or invent syntax. |
| 19 | Create an architecture diagram with 35 services; do not shrink labels to make everything fit. | Simplifies the node-label strategy, groups layers, considers scene splitting, avoids overcrowding and `W_TOO_CROWDED`, and still includes the requested scope. |
| 20 | Make a dark-mode diagram for a checkout flow that remains readable for color-blind viewers. | Uses strong contrast, words/shapes/line patterns in addition to color, clear branch labels, and no red/green-only meaning. |
| 21 | Create a four-scene explainer about how a library book gets borrowed, returned, and reshelved. Make the visuals feel specific to the subject, not like generic cards connected by arrows. | Builds a coherent, library-specific visual idea across four purposeful scenes; varies scene composition to match each beat; keeps the process accurate and readable; avoids decorative motifs unrelated to the subject. |
