# AI Rental Property Inspection Report Generator

This tool automates the creation of a "plaatsbeschrijving" (rental property inspection report) by using the OpenAI Vision API to analyze property photos for wear, tear, and damage. It processes images locally, strips metadata, renames them, and generates a professional Dutch DOCX and PDF report.

## Features
- **Automated Analysis:** Uses `gpt-4o-mini` to classify damage, normal wear and tear, severity, and provides a confidence score.
- **Smart Renaming:** Automatically renames photos to a consistent `room_description_index.jpg` format.
- **Duplicate Detection:** Prevents processing the same image twice using perceptual hashing.
- **Privacy First:** Strips EXIF GPS metadata from processed photos and resizes them for API efficiency.
- **Report Generation:** Outputs a ready-to-use, legally neutral PDF report grouped by room, with a final summary.

## Directory Structure
- `inspection_app/input_photos/`: **Place all your raw property photos here** before running the script.
- `inspection_app/output/`: Contains the generated JSON data, DOCX/PDF reports, and the newly renamed photos.
- `inspection_app/prompts/`: Contains the Dutch prompt used for the OpenAI Vision model.

## Setup & Usage

1. **Environment Variables:** 
   Ensure you have an `.env` file in this root directory with your OpenAI API key:
   ```env
   OPENAI_API_KEY=your_openai_api_key_here
   ```

2. **Run the Script:**
   Navigate into the app directory, activate the virtual environment, and execute the script:
   ```bash
   cd inspection_app
   source .venv/bin/activate
   python app.py
   ```

3. **View Results:** 
   Once the script completes, check `inspection_app/output/` for your finalized `inspection_report.pdf` and `inspection_report.docx`.
