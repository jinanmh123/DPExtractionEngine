# DPExtractionEngine
A Python-based Engine for extracting datapoints from PDF files

> [!NOTE]
> 1. This engine will be made purely using python and python libraries, without any external 3rd party solutions
> 2. PDF files will be provided to the engine via API requests, and no PDF files will leave the infrastructure
> 3. Full API integration

# Process Overview
1. First of all, a PDF file will be sent to the engine's API endpoint, which will be created using `Flask`
2. When the PDF file is gotten by the engine, the engine will convert the PDF into images(s) using the `fitz` (PyMuPDF) python library
3. For each image, masks will be applied (using `opencv`) on pre-fixed positions relative to anchors, and so instead of the whole image, only the empty areas where the fixed-position DPs are located will be scanned
4. Once scanned, the DPs will be extracted (using `pytesseract`) and stored in a temporary SQL database using the `SQLAlchemy` python library
5. Engine accepts API calls and returns data as per API call (`Flask`)

***The GUI extension will also be added to replace/adjust each DP extracted manually (the gui will be created using `jinja` in the `flask` application)***

***Process diagram:***
```
[Start]
   |
   v
[Client Submits PDF]
   |
   v
[API Endpoint (Flask) Receives PDF]
   |
   v
[Convert PDF to Images (fitz)]
   |
   v
[Load Image and Apply Masks (opencv)]
   |
   v
[Locate Anchors and Identify DP Positions]
   |
   v
[Extract DPs (pytesseract)]
   |
   v
[Store DPs in SQL Database]
   |
   v
[API Calls to Retrieve Data]
   |
   v
[Respond with Requested Data]
   |
   v
[End]
```

# Challenges and how they will be solved in the engine:
***Challenge:*** Type 2A documents are a challenge because of poor quality, rotation or varying zoom levels.
***Solution:*** `opencv` will be used to solve these problems: noise reduction and image enhancement will be used for poor quality, deskewing for rotation, and rescaling for zoom levels.

***Challenge:*** Detection of two types of checkboxes critical for DP identification and validation.
***Solution:*** `pytesseract` will help to extract dps easily using OCR.

***Challenge:*** High Accuracy
***Solution:*** `pytesseract` is highly accurate when it comes to extraction.

# Future Integrations
In the future, an LLM such as Llama 3 will be integrated, for querying dps easily and quickly in an interactive manner.
