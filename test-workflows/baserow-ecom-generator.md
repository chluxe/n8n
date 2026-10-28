# Baserow version of the e-Com Generator storage steps

The workflow `test-workflows/workflows/260.json` replaces the Airtable storage steps from the original e-Com Generator with Baserow nodes. It keeps the data model identical (fields `Détail`, `LLM`, `Photo`, `Id`, `Photos Produits`, etc.) so you can wire it into the existing HTTP/LLM/image branches by swapping the inputs on the two `Set` nodes:

- **Préparer ligne Modèle** — feed it the prompt text and generated model image URL.
- **Préparer ligne Shooting** — feed it the shooting ID and the three clothing image URLs (or the aggregated attachments from your uploads).

## Required environment variables

Set these before importing or executing the workflow so the Baserow nodes point at the right database and tables:

- `BASEROW_DATABASE_ID` — Baserow database (application) ID.
- `BASEROW_MODEL_TABLE_ID` — table ID for the “Modèle” rows.
- `BASEROW_MODEL_ACTIVE_FIELD_ID` — field ID of the boolean flag used to find the active model (used in the filter of **Baserow > Search Active Model**).
- `BASEROW_SHOOTING_TABLE_ID` — table ID for “Shooting” rows.

## Field expectations

- Model table: fields named `Détail` (text), `LLM` (text), `Photo` (file/URL array), and a boolean field referenced by `BASEROW_MODEL_ACTIVE_FIELD_ID`.
- Shooting table: fields named `Id` (text identifier), `Photos Produits` (file/URL array), and any additional fields you want to map when updating the row.

## How to integrate with the existing workflow

1. Import `test-workflows/workflows/260.json`.
2. Replace the Airtable nodes in your current flow with the Baserow nodes from this sample:
   - Use **Baserow > Create Model** instead of the Airtable “Create a record” node.
   - Use **Baserow > Search Active Model** instead of the Airtable search node.
   - Use **Baserow > Create Shooting**, **Baserow > Update Shooting**, and **Baserow > Get Shooting** instead of the Airtable create/update/get nodes.
3. Connect the upstream data into the two `Set` nodes so they emit the exact fields you want to persist.
4. Keep the rest of the workflow (LLM chains, HTTP calls, image generation) unchanged.

The workflow uses `autoMapInputData`, so as long as your Baserow tables use the same field names as the Airtable version, the mapping will work without specifying field IDs in each node.
