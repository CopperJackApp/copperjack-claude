# Ledger spending

Read every `list_deal_ledger` page for the selected deal before computing vendor or category totals. Each page repeats **whole-deal** totals; do not add those totals together. Aggregate unique entry IDs, preserve money-in versus money-out, and convert cents to dollars only for display.

Entries are payments (money out), funding (money in) and refunds. A refund (`kind` `REFUND`) is money back on one payment: it names that payment in `refundOfEntryId` and has no payee or category of its own, so it belongs to the payment's vendor and category. A payment shows its refunded total. The whole-deal money-in total includes refunds, and money out counts payments at their full amount; do not describe all money in as funding.

Group money-out by the saved payee for “totals by vendor,” less refunds on those payments, and say when a refund reduced a vendor's total. Incoming funding is not negative vendor spending, and a refund is not funding. Do not silently merge different vendor names or treat missing account labels as a known classification. Retain archived labels on historical entries. Memos and attachments supply saved context, not verified invoice contents.

Each category names the Report row it counts toward (`reportRow`): `ACQUISITION_COSTS`, `REHAB`, `FINANCING_CARRY`, `HOLDING_CARRY`, `LOAN_POINTS`, `SELLING_COSTS`, `NOT_A_DEAL_COST`, or null while an owner or admin has not picked one. A row's Ledger total is its categories' payments less their refunds. Use these rows; do not guess a row from a category's name.

To assess rehab spending, read the saved rehab budget and total the payments in `REHAB` categories, less their refunds. The whole-deal money-out total also includes acquisition, financing and other costs. Do not compare that total to the rehab budget as if all spending were rehab. Spending in a null or `NOT_A_DEAL_COST` category counts toward no Report row; if null-row categories hold spending, show those amounts and say they are not yet assigned to a row rather than assigning them yourself.

Distinguish recorded spending so far from a forecast of total cost. Remaining budget does not establish that the work will finish within budget. Use saved remaining scope or the user's information when discussing expected future costs; label estimates and missing evidence.

The Report uses these Ledger totals: a cost row with no typed Report actual shows its Ledger total as the actual. A typed actual overrides it, so a typed Report actual can differ from the Ledger. Present that difference without inventing a reconciliation or changing either source. Use `get_deal_financials` for the actuals the Report shows. A blank Report actual is not evidence of no spending; the spending may sit in an unassigned category or may not be logged yet.

If pagination cannot finish, do not report a complete vendor total. You may quote the returned whole-deal summary with its scope and read time, while stating that the entry breakdown is incomplete. Ledger attachments contain document metadata only. Use `read_deal_document_pages` to read a saved document's contents; metadata alone does not establish what it says or whether it was paid or approved.
