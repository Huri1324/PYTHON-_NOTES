# PYTHON-_NOTES

import os
import re
import pandas as pd
from PIL import Image
import pytesseract

pytesseract.pytesseract.tesseract_cmd = (
    r"C:\Program Files\Tesseract-OCR\tesseract.exe"
)

folder = "images"

data = []

for filename in os.listdir(folder):

    if filename.lower().endswith((".jpg", ".jpeg", ".png")):

        path = os.path.join(folder, filename)

        image = Image.open(path)

        text = pytesseract.image_to_string(image)

        lines = text.splitlines()

        for line in lines:

            line = line.strip()

            # məsələn: A mağazası - 10
            match = re.search(
                r"(.+?)\s*[-:]\s*(\d+)",
                line
            )

            if match:

                store = match.group(1).strip()
                workers = int(match.group(2))

                data.append({
                    "Mağaza": store,
                    "İşçi sayı": workers,
                    "Tarix": filename
                })

df = pd.DataFrame(data)

df.to_excel(
    "worker_data.xlsx",
    index=False
)

print("Excel hazırdır.")
