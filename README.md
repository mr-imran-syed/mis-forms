# MIS forms library

Ready-made ODK forms with dashboards for [MIS in a Box](../mis-in-a-box). The control panel's **Forms** page reads this
repository's **latest release** (tag), never unreleased changes. Blank templates only: never put real data here.

```
catalogue.json                 # every form: id, name, version, description, uses_entities
dictionary/fields.json         # shared field names (the panel warns when a form's names don't match)
forms/<form_id>/form.xlsx      # the XLSForm (settings sheet: form_id = <form_id>, version)
forms/<form_id>/dashboard.json # Metabase cards as SQL; {table} becomes the form's table, e.g. p1_household_survey
```

## Add or change a form

1. Put the XLSForm in `forms/<form_id>/form.xlsx`. Use the shared names from `dictionary/fields.json`
   (e.g. `household_id`, `village`, `sex`). Check it at https://getodk.org/xlsform/.
2. Optional: add `forms/<form_id>/dashboard.json` (copy the household one). Cards count `review_state = 'approved'` rows.
3. Add the form to `catalogue.json`, or raise its `version` there and in the form's settings sheet.
4. Make a release: GitHub › Releases › Draft a new release › new tag such as `v1.1.0` › Publish.
   Panels see it within 10 minutes; existing forms show **Update**.

`uses_entities` lists entity lists (e.g. `households`) a form reads with `select_one_from_file households.csv`.
The panel creates any that are missing, with their properties, before uploading the form. Lists a form *creates*
(its `entities` sheet) are made by ODK itself.

Lists the panel fills from its Excel import (Data page), with the properties forms can filter and look up on:

| List | Properties |
| --- | --- |
| districts | state, district |
| blocks | state, district, block |
| villages | state, district, block, village, village_id |
| households | household_id, head_name, phone, village, village_id |
| members | member_id, household_id, sex, birth_year (the label is the member's name) |

Cascade pattern (copy from `household_visit`): `select_one_from_file districts.csv` with `parameters` `value=district`,
then `blocks.csv` with `choice_filter` `district=${district}`, then `villages.csv` with
`district=${district} and block=${block}` and `value=village_id`.
