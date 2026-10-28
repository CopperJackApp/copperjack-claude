# Saved calculations and Report

For “Why is my offer low on 123 Main?”, resolve the deal, then read `get_deal_calculations`. Use its saved assumptions, strategy audit, explanation, criteria and limiting rules. Explain the important returned inputs and the rule that sets the offer. Do not invent a formula or infer a cause solely from the current playbook.

The calculation result uses **USD dollars**. Its input rates use whole-number percentages; insurance uses basis points; output ratios are fractions. Other deal, Report and Ledger money fields generally use **cents**. Read the field's unit before combining values.

`source` distinguishes current saved assumptions from a locked snapshot. Template changes do not change the copy saved on this deal. Results use the current calculation model at read time; they are not historical stored calculation results.

Each strategy can pass, fail or lack enough inputs. A null suggested offer is not a $0 offer. State the relevant diagnostics and do not turn an incomplete calculation into a recommendation. On a locked deal, `locked_offer.audit` evaluates the saved price and strategy; that price is not a newly suggested maximum offer. Keep locked-price findings separate from current suggested-offer results.

The tool accepts saved deal IDs only. It cannot run hypothetical input changes. You can discuss the direction a change may have, with clear attribution, but do not label an unexecuted scenario as a CopperJack calculation. Do not estimate a new ARV from the saved ARV.

For estimate-versus-actual questions, use `get_deal_financials`. Actuals are manually recorded in Report. Ledger entries are separate records and do not fill Report blanks. Null means unrecorded; zero means recorded zero.

Some unlocked estimates are scenarios at the suggested offer, not committed budgets. Use the returned comparison basis. A missing variance is not zero variance. Positive cost variance means more spent; positive sale or profit variance means more received or earned.

Read each result's own source and comparison basis. Do not carry a basis from one read into another.

Report includes separate financing and holding carry. Combined carry retains its own meaning and is the profit input. Independently rounded components can differ from the combined figure by a cent; rental debt service appears only in combined carry. Do not force the split figures to reconcile or add them to combined carry again.

Quote the values that answer the question in ordinary financial language. Explain the source or basis when it affects the answer. Use `get_deal_rehab` for item-level scope and budget evidence.
