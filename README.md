This repository offers a suite of Jupyter notebooks designed to transform raw video or audio lectures into various forms of polished, high-quality study materials. While they can be adapted for other platforms, they are **optimized for the Kaggle environment**, leveraging its access to GPUs and straightforward data handling.

## The Tools

There are three distinct notebooks, each serving a specific purpose in the workflow:

-   ### `handouter.ipynb` → *For Visual Handouts*
    This notebook is your go-to for creating a handout that mirrors the classroom experience. It transcribes a lecture and, most importantly, **integrates the corresponding presentation slides** (from a PDF or PPTX) directly into the text where they are discussed. The output is a seamless document that combines the spoken lecture with its visual aids.

-   ### `appuntatore.ipynb` → *For Detailed, Cited Notes*
    "Appuntatore" (Italian for "note-taker") creates a rich, academic-style document. It transcribes the lecture and intelligently identifies moments when the speaker is reading from a source text. It then finds those passages in provided books or articles (in PDF format) and **inserts them as full-text, formatted citations** into the transcript. This is perfect for creating comprehensive notes where direct references are crucial.

-   ### `summarizer.ipynb` → *For Condensed Study Guides*
    This is the final step for exam preparation. It takes one or more generated handouts and a "preparation guide" (like a list of study questions) to first enrich the text, ensuring all key topics are covered. It then **condenses the material into a concise summary**, perfect for efficient review.

## How to Use Them: A Step-by-Step Guide

Follow these steps to process your lecture materials from start to finish.

### Step 1: Preparation (Before You Run)

1.  **Gather Your Files**: Collect all necessary files:
    *   Your lecture recordings (e.g., `.mp4`, `.mp3`, `.m4a`).
    *   Your presentation slides (e.g., `.pptx`, `.pdf`).
    *   Any source texts or books for citation (must be `.pdf`).

2.  **Upload to Kaggle**: Create a new Kaggle Dataset and upload all your files to it. This is how the notebooks will access your materials.

3.  **Set Up Your Secrets**: To use the AI and cloud storage features, you need to provide credentials securely.
    *   In your Kaggle notebook, go to **Add-ons > Secrets**.
    *   Add your **Gemini API Key** with the name `GEMINI_API_KEY`.
    *   Add your **Google Drive Service Account JSON key** with the name `GOOGLE_DRIVE_API_KEY`.

### Step 2: Initial Setup (Inside a Notebook)

1.  **Open a Notebook**: Choose the notebook that fits your goal (`handouter.ipynb` for slides, `appuntatore.ipynb` for citations).

2.  **Run the First Cell**: The first code cell in each notebook is a setup and verification script. When you run it, it will:
    *   Install all the required Python libraries.
    *   Securely access the API keys you stored in Kaggle Secrets.
    *   Authenticate with your Google Drive account.
    *   Perform a critical **"pre-flight check"** on your Google Drive access. It does this by uploading and immediately deleting a small test file. This ensures your credentials and folder permissions are correct *before* you start a long processing job.

    > **Note:** If this cell fails, especially the Google Drive test, the final files will only be saved locally in the `/kaggle/working/` directory and will not be uploaded to the cloud.

### Step 3: The Main Workflow (Interactive Prompts)

Once the setup is complete, run the main processing cell. The script will become interactive and prompt you for input directly in the output area.

1.  **Select Media Files**: The notebook will list all the video and audio files it found in your Kaggle dataset. You will be asked to enter the number(s) corresponding to the lecture(s) you want to process.

2.  **Select Supporting Materials**: Based on the notebook you're using, you will be prompted to select other files:
    *   **In `handouter.ipynb`**: You will be asked to select the corresponding **slide deck** (PDF or PPTX) for the lecture.
    *   **In `appuntatore.ipynb`**: You will be asked to select **"Dispense" PDFs** (handouts/lecture notes) and **"Book/Citation" PDFs** (the source texts for citations).
    *   **In `summarizer.ipynb`**: You will be asked to select the handouts you want to process and a single **"Preparation Guide" PDF** containing study questions.

3.  **Let it Run**: After you make your selections, the notebook will run automatically. It will transcribe the audio, make multiple calls to the Gemini API to structure and enrich the text, and generate the final Markdown file.

### Step 4: The Full Pipeline (Combining Notebooks)

You can chain the notebooks together for a complete workflow:

1.  **Create Initial Handouts**: Run `handouter.ipynb` or `appuntatore.ipynb` to generate your initial set of detailed notes.
2.  **Package the Output**: The generated files will be saved in `/kaggle/working/`. Create a *new* Kaggle Dataset using the output files from this directory.
3.  **Summarize for Review**: Start the `summarizer.ipynb` notebook. Add the new dataset you just created as input. When prompted, select the handouts and your preparation guide to create the final, condensed study materials.

NOTE that in-line prompts are ment to be custiomizable so to adapt them to the study material
