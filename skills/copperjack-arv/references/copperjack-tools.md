# CopperJack evidence and comp searches

These are CopperJack MCP tool names; hosts may display a server prefix. Use the live tool definitions for the current schema. This reference reflects the available definitions checked on 2026-09-13.

## Property and photos

- Reuse a known `deal_id`. Otherwise call `list_deals` with the property's address or saved alias in `address_query` and `state: "all"` when its state is unknown. Match the address before selecting a result; page with the same query/state if necessary.
- Call `get_deal({deal_id})`. Use its current `analysis_facts` for living area, beds, baths and garage, plus the remaining `property_facts` such as lot, year and property type. A missing overview field does not prove the information is unavailable elsewhere.
- Keep `above_sqft`, `below_sqft`, and `total_sqft` distinct. `working_sqft` alone does not establish an above-grade area or an above/below split. Unknown is different from zero.
- Call `list_deal_photos({deal_id})`, following any cursor with the same deal ID. Use the returned `gallery_token` with `get_deal_photo_sheet({deal_id, gallery_token, sheet_number})`; inspect individual details with `get_deal_photo({deal_id, gallery_token, photo_id})`. Send each saved-photo or saved-sheet call individually. Refresh the list if the token expires or the gallery changes.
- Inspect the returned images, not just captions or counts. Contact sheets efficiently cover the home; full photos help resolve meaningful details. 

Money in `get_deal` and `get_deal_rehab` is USD cents. Normalize units before calculating or displaying amounts; do not assume every MLS or map price uses those units. Use declared units/field definitions for each response. If a price's units are unresolved, verify them before relying on that price. Saved estimates and acquisition figures are not comp evidence.

## Start a private search

`search_comps` requires `deal_id` and **all four** Tune settings:

| Parameter | Allowed values |
|---|---|
| `radius_miles` | `0.25`, `0.5`, `1`, `2`, `5`, `10` |
| `sold_within_months` | `3`, `6`, `12`, `24` |
| `similarity` | `"tight"`, `"balanced"`, `"loose"` |
| `per_status_cap` | `4`, `6`, `8`, `10`, `12` |

A reasonable starting point for a typical neighborhood is 0.5 miles, 6 months, balanced similarity, and a cap of 8. Adapt to the property's setting and any requested search limits. These controls do not include a renovation-only filter, custom square-footage range, or arbitrary status filter; screen returned candidates using their actual facts and images.

Each call creates a new private search. Retain its `search_id` and poll **that ID** with `get_comp_search({search_id})`:

- **PREPARING:** Follow the returned polling hint; do not submit another search to poll.
- **READY:** Inspect the frozen subject, settings, acquisition/selection counts, sold and pending comparisons, and photo-evidence status.
- **FAILED:** Report the actual limitation, use other credible evidence already available, and do not automatically resubmit the failed search.

Results are bounded samples, not an exhaustive market census. A short result can reflect acquisition, usability, matching, or selection caps. It does not by itself establish that the market lacks comps. Private searches do not alter the saved workspace or automatically promote results.

## Refine for a reason

Choose the change that addresses the observed limitation:

- **Useful matches held by the cap:** Raise `per_status_cap` before sacrificing location or comparability.
- **Few matching sales, no binding cap:** Expand time or radius to find the closest competing homes. For example, six months may preserve neighborhood relevance better than a much wider radius.
- **Structurally unusual subject:** Relax `similarity` where appropriate, then inspect the newly introduced differences.
- **Mostly dated homes:** Inspect additional candidates and expand deliberately if needed. Changing Tune cannot guarantee renovated inventory.
- **Already strong evidence:** Stop; more weak comps do not improve the estimate.

Change one relevant control at a time when practical, keeping the other Tune settings explicit. Record what changed and whether it added useful evidence. End expansion when the evidence supports the conclusion, successive searches add no useful evidence, available limits are reached, or a tool fails. Do not search repeatedly for a preferred price or confidence label.

Deduplicate transactions across searches. Retain each candidate's originating `search_id` and IDs for media access; old results stay frozen. When comparing results from different searches, ensure their subject facts describe the same property being valued.

## Inspect comparisons

- `get_comp_map({search_id, comp_ids?})` returns a subject/comp map. Use IDs belonging to that ready search; inspect the image and legend together. Note omitted coordinates. A pin does not establish noise, school quality, flood exposure, or other features the evidence does not show.
- `get_comp_photo_sheet({search_id, comp_id, sheet_number})` returns numbered comparison images. Use `get_comp_photo({search_id, photo_id})` to inspect decisive details at full size. Keep IDs with the search that returned them.
- Photo evidence may be terminal `partial-complete`; unavailable tiles are missing evidence, not proof that a room or feature is absent. Do not poll a ready search indefinitely for missing photos.

Use sold prices as transaction evidence. Pending prices remain listing prices unless a completed sale is separately verified. If active, withdrawn, or expired listings are supplied through other evidence, label them separately; this search API returns sold and pending comparisons.

`get_deal_rehab({deal_id})` is available if a closing footnote needs the documented scope; its budget and line-item totals are not valuation inputs.
