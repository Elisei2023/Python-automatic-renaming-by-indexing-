import tabula
import pandas as pd
import os

"""Funcție pentru redenumirea fișierelor de imagini"""
def rename_images(coduri, image_folder):
    for cod in coduri:
        # Găsește toate fișierele în folderul de imagini
        files = sorted(os.listdir(image_folder))

        """Filtrează doar fișierele care sunt imagini """
        image_files = [f for f in files if f.lower().endswith(('.png', '.jpg', '.jpeg', '.tiff', '.bmp', '.gif'))]

        for i, image_file in enumerate(image_files):

            new_name = f"{cod}.{i+1}{os.path.splitext(image_file)[1]}"

            """Construim calea completă pentru fișierele originale și redenumit"""
            old_path = os.path.join(image_folder, image_file)
            new_path = os.path.join(image_folder, new_name)

            # Redenumește fișierul
            os.rename(old_path, new_path)
            print(f"Renamed {old_path} to {new_path}")

"""Specifică calea către fișierul PDF"""
pdf_path = "D:/REDEVENTA/CentralizatorRedeventeDetaliat 1-8.pdf"

"""Extrage tabelele din toate paginile PDF-ului"""
tables = tabula.read_pdf(pdf_path, pages="all", multiple_tables=True)

"""Initializează o listă pentru a stoca toate codurile"""
all_coduri_tranzactie = []

"""Parcurge toate tabelele extrase din pagini"""
for table in tables:

    if "Cod tranzactie" in table.columns:
        # Extrage codurile și le adaugă la listă
        coduri = table["Cod tranzactie"].dropna().tolist()
        all_coduri_tranzactie.extend(coduri)


df_coduri = pd.DataFrame(all_coduri_tranzactie, columns=["Cod tranzactie"])

"""Afișează codurile extrase"""
print(df_coduri)


"""Redenumirea fișierelor de imagini"""
image_folder = r"D:\REDEVENTA\poze"  # Calea către folderul cu poze
rename_images(all_coduri_tranzactie, image_folder)
