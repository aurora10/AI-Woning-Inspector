# AI Rental Property Inspection Report Generator
## Exhaustive Step-by-Step Instructions for Codex

Goal:

Build a Python script that:

1. Reads all property photos from a folder
2. Uses a Vision AI model to analyze each image
3. Detects:
   - damage
   - wear and tear
   - stains
   - cracks
   - scratches
   - humidity
   - paint condition
   - flooring condition
4. Renames each image automatically
5. Generates structured JSON
6. Generates a professional PDF report in Dutch
7. Creates a legally neutral "plaatsbeschrijving" style report

NO UI required.

The application is a one-off offline batch processing tool.

---

# 1. Technology Stack

Use:

- Python 3.11+
- OpenAI Vision API (GPT-4.1 or newer)
- python-docx
- docx2pdf
- Pillow
- reportlab (optional)
- pathlib
- tqdm

---

# 2. Project Structure

Create the following structure:

```text
inspection_app/
│
├── input_photos/
│
├── output/
│   ├── renamed_photos/
│   ├── inspection_report.docx
│   ├── inspection_report.pdf
│   └── inspection_data.json
│
├── prompts/
│   └── vision_prompt.txt
│
├── app.py
├── requirements.txt
└── .env
3. Install Dependencies

Create requirements.txt

openai
python-docx
docx2pdf
pillow
python-dotenv
tqdm
reportlab

Install:

pip install -r requirements.txt
4. Configure Environment Variables

Create .env

OPENAI_API_KEY=YOUR_KEY_HERE
5. Vision Analysis Requirements

The Vision model MUST:

analyze every photo independently
produce objective legal-style descriptions
avoid emotional wording
avoid assumptions
distinguish:
normal wear and tear
actual damage
unclear observations

The model MUST answer in Dutch.

6. Vision Prompt

Create:

prompts/vision_prompt.txt

Je analyseert foto's van een huurwoning voor een plaatsbeschrijving.

Beschrijf uitsluitend objectief zichtbare zaken.

Detecteer indien zichtbaar:
- schade
- slijtage
- krassen
- scheuren
- vochtplekken
- verkleuring
- vlekken
- beschadigde verf
- beschadigde vloer
- beschadigde muren
- beschadigde meubels
- beschadigde deuren of ramen

BELANGRIJK:
- Schrijf neutraal en professioneel.
- Maak geen aannames.
- Gebruik juridisch neutrale taal.
- Vermijd emotionele formuleringen.
- Indien iets normale slijtage lijkt, vermeld dit expliciet.
- Indien onduidelijk, vermeld dat.

Antwoord ALTIJD in geldig JSON formaat.

JSON structuur:

{
  "room": "kamernaam",
  "title": "korte bestandsnaam",
  "description": "korte objectieve beschrijving",
  "category": "normale slijtage | schade | onduidelijk",
  "severity": "laag | gemiddeld | hoog"
}
7. Expected AI Output Example
{
  "room": "keuken",
  "title": "krassen_keukenkast",
  "description": "Lichte krassen zichtbaar op de onderste keukenkasten. Dit lijkt overeen te komen met normale slijtage.",
  "category": "normale slijtage",
  "severity": "laag"
}
8. File Renaming Logic

The script MUST rename files automatically.

Filename format:

ROOM_SHORTDESCRIPTION_INDEX.jpg

Example:

keuken_krassen_kast_001.jpg
badkamer_vochtplek_002.jpg
woonkamer_verkleuring_muur_003.jpg

Rules:

lowercase only
replace spaces with underscores
remove special characters
max 40 characters
preserve extension

If duplicate filename exists:

append incremental suffix

Example:

keuken_krassen_001.jpg
keuken_krassen_002.jpg
9. Processing Pipeline

The script workflow MUST be:

1. Load all images from input_photos
2. Send each image to Vision model
3. Receive structured JSON
4. Validate JSON
5. Rename image
6. Copy renamed image to output/renamed_photos
7. Save metadata into inspection_data.json
8. Generate DOCX report
9. Convert DOCX to PDF
10. OpenAI Vision API Implementation

Use the OpenAI Responses API.

Model:

gpt-4.1-mini
OR
gpt-4.1

Image input:

base64 encode image
OR
direct file upload

The implementation MUST:

retry failed requests
validate JSON response
handle malformed AI output gracefully

Use temperature:

temperature=0.1

to ensure consistent descriptions.

11. JSON Storage Structure

Final JSON MUST contain ALL processed photos.

Example:

[
  {
    "original_filename": "IMG_8821.jpg",
    "new_filename": "keuken_krassen_kast_001.jpg",
    "room": "keuken",
    "description": "Lichte krassen zichtbaar op de onderste keukenkasten. Dit lijkt overeen te komen met normale slijtage.",
    "category": "normale slijtage",
    "severity": "laag"
  }
]
12. PDF/DOCX Report Requirements

The report MUST be professional and legally neutral.

Language:

Dutch

Title:

PLAATSBESCHRIJVING – VISUELE VASTSTELLING

Include:

current date
sequential numbering
room grouping
image thumbnails
descriptions
category
severity
13. Report Layout

Structure:

# PLAATSBESCHRIJVING – VISUELE VASTSTELLING

Datum: DD/MM/YYYY

## Keuken

### Foto 1

[IMAGE]

Beschrijving:
Lichte krassen zichtbaar op de onderste keukenkasten. Dit lijkt overeen te komen met normale slijtage.

Categorie:
Normale slijtage

Ernst:
Laag

Repeat for every photo.

14. Grouping by Room

The final report MUST group observations by room.

Example order:

inkomhal
woonkamer
keuken
badkamer
slaapkamer
terras
garage
overige

If room uncertain:

place in overige
15. Image Handling

Before sending images to AI:

resize large images
preserve aspect ratio
max dimension:
1600px

This reduces API cost.

Use Pillow.

16. Error Handling

The application MUST handle:

invalid images
corrupted files
API failures
malformed JSON
duplicate filenames
missing EXIF
unsupported extensions

Supported extensions:

.jpg
.jpeg
.png
.webp
17. Logging

Create console logging:

[1/52] Processing IMG_8821.jpg...
[OK] Renamed to keuken_krassen_kast_001.jpg

Show progress with tqdm.

18. DOCX Generation

Use python-docx.

The DOCX MUST include:

headings
inserted images
consistent formatting
page breaks between rooms

Image width:

approx 12 cm

Descriptions:

below image
19. PDF Conversion

After DOCX creation:

Convert automatically to PDF.

Use:

docx2pdf

Output:

output/inspection_report.pdf
20. Legal Tone Requirements

VERY IMPORTANT.

Descriptions MUST:

GOOD:

Lichte slijtage zichtbaar op de muur nabij de deur.

BAD:

De eigenaar heeft de woning slecht onderhouden.

Never:

assign blame
speculate
exaggerate
diagnose causes

Only describe visible observations.

21. Recommended Additional Metadata

Include optional metadata:

{
  "timestamp": "...",
  "image_width": 1920,
  "image_height": 1080
}
22. Optional Improvement (Recommended)

Add duplicate image detection.

Reason:

tenants often accidentally photograph same area twice

Use perceptual hashing:

imagehash

If duplicate detected:

skip image
OR
mark as duplicate
23. Optional Improvement: Confidence Scores

Ask AI to return:

"confidence": 0.92

Low confidence items can later be manually reviewed.

24. Optional Improvement: Summary Section

At end of report generate summary:

SAMENVATTING

Totaal aantal foto's: 52

Normale slijtage:
41

Schade:
7

Onduidelijk:
4
25. Security & Privacy Requirements

The application MUST:

process locally except API calls
never upload report publicly
never store data externally
never include GPS metadata in report

Strip EXIF GPS metadata if present.

26. Final Deliverables

The final application MUST produce:

output/
│
├── renamed_photos/
├── inspection_data.json
├── inspection_report.docx
└── inspection_report.pdf
27. Expected Runtime

Approximate processing time:

50 images
GPT-4.1-mini
~3–8 minutes

depending on internet speed.

28. Coding Standards

The implementation MUST:

use pathlib instead of os.path
separate functions cleanly
use type hints
use dataclasses where appropriate
avoid hardcoded paths
be executable from terminal

Command:

python app.py
29. Suggested Internal Functions

Implement:

load_images()
resize_image()
analyze_image()
validate_ai_response()
sanitize_filename()
rename_image()
save_json()
generate_docx()
convert_to_pdf()
30. Final Objective

The final system should create a professional Dutch rental inspection report that:

links every image to its description
documents visible wear and tear
minimizes future disputes about rental deposit deductions
creates legally neutral evidence of property condition