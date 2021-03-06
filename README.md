# Niffler: A DICOM Framework for Machine Learning Pipelines and Processing Workflows.

Niffler is an efficient DICOM Framework for machine learning pipelines and processing workflows on metadata. It facilitates efficient transfer of DICOM images on-demand and real-time from PACS to the research environments, to run processing workflows and machine learning pipelines.

Niffler enables receiving DICOM images real-time as a data stream from PACS as well as specific DICOM data based on a series of DICOM C-MOV queries. The Niffler real-time DICOM receiver extracts the metadata free of PHI as the images arrive, store the metadata in a Mongo database, and deletes the images nightly. The on-demand extractor reads a CSV file provided by the user (consisting of EMPIs, AccessionNumbers, or other properties), and performs a series of DICOM C-MOVE requests to receive them from the PACS, without manually querying them. Niffler also provides additional features such as converting DICOM images into PNG images, and perform additional computations such as computing scanner utilization and finding scanners with misconfigured clocks.


# Niffler Modules

Niffler consists of a modular architecture that provides its features. Each module can run independently. Niffler core (cold-extraction, meta-extraction, and png-extraction) is built with Python-3.6. Niffler application layer (app-layer) is built with Java and Javascript.

## cold-extraction

Parses a CSV file consisting of EMPIs, AccessionNumbers, or Study/Accession Dates, and performs a series of DICOM C-MOVE queries (often each C-MOVE following a C-FIND query) to retrieve DICOM images retrospectively from the PACS.

## meta-extraction

Receives DICOM images as a stream from a PACS and extracts and stores the metadata in a metadata store (by default, MongoDB), deleting the received DICOM images nightly.

## png-extraction

Converts a set of DICOM images into png images, extract metadata in a privacy-preserving manner. The extracted metadata is stored in a CSV file, along with the de-identified PNG images. The mapping of PNG files and their respective metadata is stored in a separate CSV file.

## app-layer

The app-layer (application layer) consists of specific algorithms. The app-layer/src/main/scripts consists of Javascript scripts such as scanner clock calibration. The app-layer/src/main/java consists of the the scanner utilization computation algorithms developed in Java.


# Configuring Niffler

## Configure PACS

Make sure to configure the PACS to send data to Niffler's host, port, and AE_Title. Niffler won't receive data unless the PACS allows the requests from Niffler (host/port/AE_Title).

## Install Dependencies

To use Niffler, first, install the dependencies.

$ pip install -r requirements.txt

Also install DCM4CHE from https://github.com/dcm4che/dcm4che/releases

For example,

$ wget https://sourceforge.net/projects/dcm4che/files/dcm4che3/5.22.5/dcm4che-5.22.5-bin.zip/download -O dcm4che-5.22.5-bin.zip

$ sudo apt install unzip

$ unzip dcm4che-5.22.5-bin.zip

Make sure Java is available, as DCM4CHE and Niffler Application Layer require Java to run.

You should first configure the operating system's [mail](https://www.javatpoint.com/linux-mail-command) client for the user that runs Niffler modules, if you have enabled mail sender for any of the modules through their respective config.json. This is a one-time configuration that is not specific to Niffler.

## Deploy Niffler

Then checkout Niffler source code.

$ git clone https://github.com/Emory-HITI/Niffler.git

$ cd Niffler

The master branch is stable whereas the dev branch has the bleeding edge.

The Java components of Niffler Application Layer are managed via Apache Maven 3.

$ mvn clean install

Please refer to each module's individual README for additional instructions on deploying and using Niffler for each of its components.



# Citing Niffler

If you use Niffler in your research, please cite the below papers:

* Pradeeban Kathiravelu, Puneet Sharma, Ashish Sharma, Imon Banerjee, Hari Trivedi, Saptarshi Purkayastha, Priyanshu Sinha, Alexandre Cadrin-Chenevert, Nabile Safdar, Judy Wawira Gichoya. **A DICOM Framework for Machine Learning Pipelines against Real-Time Radiology Images.** arXiv preprint [arXiv:2004.07965](http://arxiv.org/abs/2004.07965) (2020).

* Kathiravelu, P., Sharma, A., & Sharma, P. (2021). **Understanding Scanner Utilization With Real-Time DICOM Metadata Extraction.** IEEE Access, 9, 10621-10633. https://doi.org/10.1109/ACCESS.2021.3050467
