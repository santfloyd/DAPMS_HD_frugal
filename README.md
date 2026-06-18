DAPMS: Data Acquisition Process Management System for Heritage Documentation
A systemic, relational database architecture and automated Python ingestor for managing complex 3D Heritage Documentation (HD) projects.

This repository contains the database schema, ingestion scripts, and documentation for the DAPMS (Version 4.6), a robust digital infrastructure designed to track data provenance, manage fieldwork paradata, and seamlessly bridge Geomatics (GIS) with Heritage Building Information Modeling (HBIM).

This system was developed and validated during the multi-sensor documentation of the Capilla de Siecha case study.

📖 About the Project
During complex 3D digitization workflows, tracking data provenance becomes essential as project complexity rapidly escalates due to multi-sensor integrations, high data volumes, and intricate object geometries.

The DAPMS addresses this complexity by treating heritage documentation as a dynamic system. It utilizes a "Core vs. Satellite" relational database structure to interlink baseline metadata, fieldwork paradata, and laboratory processing specifications. Ultimately, this architecture establishes a highly interoperable master nexus—one source to rule them all—providing the systemic data required for heritage collections management, HBIM elaboration, GIS analysis, and intra-project knowledge transfer.

🌟 Key Features
Frugal & Accessible Data Entry: A spreadsheet-based Python ingestor allows non-technical staff to insert complex data via standard .ods or .xlsx files, shielding them from complex SQL queries.

Semantic Web Ready: Automatically maps laboratory processes and baseline data to CIDOC-CRM ontologies (e.g., E27_Site, E16_Measurement, E11_Modification) without manual intervention.

Geospatial & 3D Integration: Utilizes PostGIS spatial tiles (pointcloud_patches) to segment massive 3D geometries into manageable bounding boxes for software like QGIS, BlenderBIM, or Rhinoceros 3D.

Protected Provenance: Strict relational constraints (ON DELETE CASCADE) and idempotent Python scripts ensure that foundational elements (like camera sensors or processing steps) cannot be duplicated or accidentally deleted.

FAIR & IIIF Compliant: Designed to adhere to FAIR data principles and support International Image Interoperability Framework (IIIF) 3D specifications.

🗂️ Repository Structure
sql/DAPMS_schema_v4.6.sql: The master PostgreSQL/PostGIS database schema. Run this first to build the tables, views, and constraints.

python/dapms_ingestor.py: The automated Python pipeline that reads the spreadsheet and safely maps it into the relational database.

python/dapms_deleter.py: A utility script for safely removing specific records (and their downstream dependencies) using human-readable codes.

templates/DAPMS_Data_Entry.ods: The blank template spreadsheet designed for non-technical data entry.

⚙️ Prerequisites & Installation
1. Database Setup
You will need an active installation of PostgreSQL (v12 or higher) with the PostGIS and PointCloud extensions enabled.

Create a new database in pgAdmin (e.g., dapms_capilla).

Open the Query Tool and execute the DAPMS_schema_v4.6.sql script to generate the architecture.

2. Python Environment
The ingestion pipeline requires Python 3.8+ and a few standard data science libraries. Install the dependencies using your terminal:

Bash
pip install pandas psycopg2-binary odfpy openpyxl
(Note: odfpy is required for reading LibreOffice/OpenOffice .ods files, while openpyxl handles Excel .xlsx files).

🚀 How to Use the System
Step 1: Data Entry
Open the DAPMS_Data_Entry.ods template and fill in your project data across the respective tabs (Projects, Cameras, Assets, Acquisitions, Processing_Lab, Image_Specs).

Step 2: Configure the Database Connection
Open dapms_ingestor.py and update the DB_CONFIG dictionary at the top of the file with your local PostgreSQL credentials:

Python
DB_CONFIG = {
    "dbname": "dapms_capilla",
    "user": "postgres",
    "password": "your_secure_password",
    "host": "localhost",
    "port": "5432"
}
Step 3: Run the Ingestor
Run the script from your terminal. The script is 100% idempotent, meaning you can run it multiple times safely. It will update existing records, skip duplicates, and halt with a helpful error message if you left a mandatory field blank in the spreadsheet.

Bash
python dapms_ingestor.py
Step 4: Correcting Mistakes (The Deleter)
If you realize a specific processing step or asset was documented incorrectly, do not try to manually delete it in pgAdmin. Open dapms_deleter.py, specify the human-readable code (e.g., PROC-RECON-01), and run the script. It will safely remove the record and cascade the deletion to all associated semantic mappings and EXIF data.

Bash
python dapms_deleter.py
📚 Academic Citation
If you use this architecture or code in your own research, please cite the corresponding paper:

[Your Name] (2026). [Exact Title of Your Thesis/Paper] [University/Institution Name]. [Link or DOI if applicable]

📄 License
This project is licensed under the MIT License - see the LICENSE file for details. Open-science methodologies return agency and control to researchers!
