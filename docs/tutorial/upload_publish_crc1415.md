# Using NOMAD in the CRC1415

In this tutorial, we follow the entire workflow for uploading, sharing and publishing research data in NOMAD via the graphical user interface (GUI). We will focus on the [NOMAD-CRC1415-Plugin](https://github.com/CRC1415/NOMAD-CRC1415-Plugin) for collecting measurement data of a sample and the corresponding workflow. By the end of the tutorial, we will set-up an upload and sharing data within the [CRC1415](https://tu-dresden.de/mn/chemie/sfb1415).

!!! warning "Before you proceed, make sure you have read the tutorial about [Upload and publish your own data](../tutorial/upload_publish.html)"

---

## What you will learn

In this tutorial, you will learn how to:

1. Upload raw research data to NOMAD and organize it using uploads
2. View the entries that NOMAD generates from your files and check their processing status
3. Share uploads with collaborators and manage access permissions
4. Publish uploads and understand the restritions


---

## Before you begin

This tutorial requires no prior experience with NOMAD except the basic concepts explained in the [previous tutorial](../tutorial/upload_publish.html).

Before starting, make sure you have the following:

1. **ZIH@TUD user account for NOMAD Oasis CRC1415**  
    In order to upload data into NOMAD Oasis for the CRC1415, a user account is required.
    You need to have ZIH-login. If you don't have one, reach out to your contact person [requesting a guest login](https://faq.tickets.tu-dresden.de/otrs/public.pl?Action=PublicFAQZoom;ItemID=495).

2. **Example files available on your local machine**  
    This tutorial uses provided example data files for:
    - [X-ray diffraction data file (RAW)](https://github.com/CRC1415/NOMAD-CRC1415-Plugin/raw/refs/heads/main/tests/datacrc1415plugin/test_file_XRD.raw){:target="_blank" rel="noopener"},
    - [Electron microscopy data (TIF)](https://github.com/CRC1415/NOMAD-CRC1415-Plugin/raw/refs/heads/main/tests/datacrc1415plugin/test_file_SEM_01.tif){:target="_blank" rel="noopener"},
    - [Raman spectroscopy data (TVB)](https://github.com/CRC1415/NOMAD-CRC1415-Plugin/raw/refs/heads/main/tests/datacrc1415plugin/test_file_Raman_TVB_10Frames.tvb){:target="_blank" rel="noopener"},
    - [Crystallographic Information File (CIF)](https://github.com/CRC1415/NOMAD-CRC1415-Plugin/raw/refs/heads/main/tests/datacrc1415plugin/test_file_Overview_CIF.cif){:target="_blank" rel="noopener"},
    - [README file (MD)](https://zenodo.org/records/14848834/files/README.md?download=1){:target="_blank" rel="noopener"}.

---

## Login to NOMAD Oasis CRC1415

A ZIH-login user account is required if you want to upload, share, publish, or analyze your data.

**Use the arrow buttons ⬅️➡️ below to slide through the steps and login with your ZIH-account into NOMAD Oasis CRC1415.**
<div class="image-slider" id="slider1">
    <div class="nav-arrow left" id="prev">←</div>
    <img src="./images_crc1415/login_01.png" alt="Login" class="active">
    <img src="./images_crc1415/login_02.png" alt="ZIH Authentication">
    <img src="./images_crc1415/login_03.png" alt="Login Successful">
    <div class="nav-arrow right" id="next">→</div>
</div>

---

## Create new upload

The uploads exist in the *Your uploads* page. Here you can view a list of all your uploads with their relevant information. You can also create new uploads or add an example upload prepared by others.

**Use the arrow buttons ⬅️➡️ below to follow the steps for creating your first upload.**

<div class="image-slider" id="slider1">
    <div class="nav-arrow left" id="prev1">←</div>
    <img src="images_crc1415/upload_01.png" alt="Navigate to `Your uploads` page by hovering over the Publish menu, then clicking on uploads" class="active">
    <img src="images_crc1415/upload_02.png" alt="Create a new upload by clicking on the blue button">
    <img src="images_crc1415/upload_03.png" alt="Edit the name of your upload">
    <img src="images_crc1415/upload_04.png" alt="Save the name of your upload">
    <div class="nav-arrow right" id="next1">→</div>
</div>

??? info "Two different views of the **upload** page"
    At the very top of the upload page, you can toggle between two different views for this page:

    - **Overview:** This view includes all the tools needed to manage your upload, along with guided steps to start uploading your files to NOMAD. It will also show all the processed files (entries) that you will upload.
    ![a screenshot of the uploads overview page](images/upload_publish_7.png)
    - **Files:** This view shows all the files included in the upload, whether they are raw files or processed files. You can also organize these files into folders as needed.
    ![A screenshot of the uploads files page](images/upload_publish_8.png)

??? info "Icons on the upload overview"

    At the top of the `OVERVIEW` tab, you will find several icons that help you to manage your upload:

    ![Top fields in uploads page](images/top_fields_uploads.png){:.screenshot}

    The name of the upload can be modified by clicking on the pen icon :fontawesome-solid-pen:. The other icons are used as follows:

    :fontawesome-solid-user-group: **Manage members:** Allows users to invite collaborators by assigning co-authors and reviewers roles.

    :fontawesome-solid-cloud-arrow-down: **Download files:** Downloads all files present in the upload.

    :fontawesome-solid-rotate-left: **Reload:** Reloads the uploads page.

    :fontawesome-solid-rotate: **Reprocess:** Triggers the uploaded data to be processed again.

    :fontawesome-solid-angle-left::fontawesome-solid-angle-right: **API:** Displays a GET request url and corresponding JSON response demonstrating how to access the entries of the upload via the NOMAD API and the expected result, respectively.
    <!-- See [Filtering and Querying](../filtering_and_querying/overview.md) for more information. -->
    <!-- TODO  Add API to glossary -->

    :fontawesome-solid-trash: **Delete the upload:** Deletes the upload permanently.

??? info "Components of the upload overview"
    The remainder of the uploads page is divided into five segments, each presenting a step in the uploading and publishing process:

    `1. Prepare and upload your files:` displays the files and folder structure of the upload. You can add a `README.md` file to the root directory and its contents will be shown above this section.

    `2. Process data:` shows the processed data and the generated [entries](../reference/glossary.md#entry) in NOMAD.

    `3. Edit visibility and access:` allows users to make the upload public or share it with specific users before publishing.

    `4. Edit author metadata:` allows users to edit certain metadata fields from all entries recognized in the upload. This includes _comments_, where you can add as much extra information as you want, _references_, where you can add a URL to your upload (e.g., an article DOI), and _datasets_, where you can link the uploaded data to other uploads to define a larger-scale organizational structure (see [Group entries into a dataset](#group-entries-into-a-dataset) below.)

    `5. Publish:` lets users publish data with or without an embargo (i.e., a waiting period before the data are publicly visible).

---

## Share and publish uploads

Uploads in NOMAD can be shared or published. When an upload is shared or published, all entries and files contained within it are also shared or published.

??? info "What is the difference between sharing and publishing an Upload?"
    **Sharing an upload** allows you to grant access to colleagues or collaborators while working on it.

    - This facilitates collaboration on projects or enables progress reviews during the project.
    - You can invite individuals with either read/write access (coauthors) or read-only access (reviewers).

    **Publishing an upload** makes it searchable, findable, and accessible on NOMAD for everyone.

    - Once published, the upload becomes immutable, meaning its contents can no longer be modified.
    - You also have the option to publish with an embargo period, temporarily restricting public access until the embargo expires.

A NOMAD upload can have four states based on sharing and publishing:

|Status    | Icon                                                                           | Description                                                           |
|----------|--------------------------------------------------------------------------------|-----------------------------------------------------------------------|
|Private   |<img src="images/icon_unpublished.png" alt="Icon of private upload" width="20"> |The upload is private and is visible to the uploader only.             |
|Shared    |<img src="images/icon_shared.png" alt="Icon of shared upload" width="20">       |The upload is accessible to selected users but not publicly available. |
|Published |<img src="images/icon_published.png" alt="Icon of published upload" width="20"> |The upload is publicly available to everyone.                          |
|Visible   |<img src="images/icon_visible.png" alt="Icon of visible upload" width="20">     |The upload is unpublished but accessible to everyone.                  |

You can manage upload sharing in the *Edit upload members* menu. To access it, click on <img src="images/edit_upload_members_icon.png" alt="Edit upload members Icon" width="20"> available at the top of the upload page.

Alternatively, you can click the `EDIT UPLOAD MEMBERS` button below the list of entries on your upload page.

??? task "Share your upload"
    <img src="images/icon_shared.png" alt="Icon of shared upload" width="30">

    **Step 1:** Open the *Edit upload members* window, by clicking on the **EDIT UPLOAD MEMBERS** button.

    **Step 2:** Start typing the name of the NOMAD user you want to share the upload with. A list of matching users will appear—select the correct name from the list.

    **Step 3:** Assign a role to the user by selecting either Coauthor or Reviewer from the dropdown menu.
        - Coauthors can edit and add files to the upload.
        - Reviewers can only view the contents of the upload (no editing rights).

    **Step 4:** Click on *submit*.

    ![screenshots of the steps of sharing your upload](images/sharing_an_upload.png)

??? task "Make your upload visible to everyone"
    <img src="images/icon_visible.png" alt="Icon of shared upload" width="30">

    To make your upload visible to everyone on NOMAD, simply check the box in the *Edit visibility and access* section, located under the list of your entries.

    This setting allows all users, including guests without an account, to view the upload even before it is published.

    You can still modify the upload’s access settings and edit its contents while it remains visible.

    ![screenshot of the steps of making an upload visible](images/making_an_upload_visible.png)
??? task "Publish your upload"
    <img src="images/icon_published.png" alt="Icon of shared upload" width="30">

    !!! warning "Once an upload is published, it cannot be deleted, and the files and entries cannot be changed"

    **Step 1:** Select an embargo period (if needed) from the dropdown menu, located in the *publish* section at the bottom of the upload page.

    If you would like to publish immediately, select *No embargo*.

    **Step 2:** Click on the **PUBLISH** button.

    **Step 3:** A prompt for confirmation appears on the screen. Click on PUBLISH.

    ![screenshots of the steps to publish an upload](images/publishing_an_upload.png)

    Having an embargo on your data means:

    - Raw files and entries remain hidden, except to you and users you share the data with.
    - Some metadata (e.g., chemical formula, system type, spacegroup) will be public.
    - Embargoed entries can be added to datasets and be assigned DOIs.
    - The embargo can be lifted earlier than the assigned duration by the user.

    The following image shows an example of an embargoed upload and the option to lift the embargo by clicking the **LIFT EMBARGO** button.

    ![screenshot of an embargoed upload](images/embargoed_upload.png)

---

## Add files to an upload

Let's start adding files to your NOMAD upload. We will explore some examples:

1. X-ray diffraction data file (RAW)
2. Electron microscopy data (TIF)
3. Raman spectroscopy data (TVB)
4. Crystallographic Information File (CIF)
5. ReadMe file in MarkDown (MD)

Files can be added to an upload individually, or you can group them into a compressed file in `.zip` or `.tar` formats.

### Upload miscellaneous files

??? example "Download the example files for this exercise"
    - [X-ray diffraction data file (RAW)](https://github.com/CRC1415/NOMAD-CRC1415-Plugin/raw/refs/heads/main/tests/datacrc1415plugin/test_file_XRD.raw){:target="_blank" rel="noopener"},
    - [Electron microscopy data (TIF)](https://github.com/CRC1415/NOMAD-CRC1415-Plugin/raw/refs/heads/main/tests/datacrc1415plugin/test_file_SEM_01.tif){:target="_blank" rel="noopener"},
    - [Raman spectroscopy data (TVB)](https://github.com/CRC1415/NOMAD-CRC1415-Plugin/raw/refs/heads/main/tests/datacrc1415plugin/test_file_Raman_TVB_10Frames.tvb){:target="_blank" rel="noopener"},
    - [Crystallographic Information File (CIF)](https://github.com/CRC1415/NOMAD-CRC1415-Plugin/raw/refs/heads/main/tests/datacrc1415plugin/test_file_Overview_CIF.cif){:target="_blank" rel="noopener"},
    - [README file (MD)](https://zenodo.org/records/14848834/files/README.md?download=1){:target="_blank" rel="noopener"}.
    
    Download all files to a local folder on your machine in preferred directory.

    The files have the following formats: `.raw`, `.tvb`, `.tif`, and `.cif`.

    | file name                          | format | description                                                                 |
    |------------------------------------|--------|-----------------------------------------------------------------------------|
    | test_file_XRD.raw                  | .raw   | Binary file of XRD data based on Bruker `.raw` format                       |
    | test_file_Raman_TVB_10Frames.tvb   | .tvb   | Binary file of Raman data based on TriVista `.tvb` format                   |
    | test_file_SEM_01.tif               | .tif   | A scanning electron microscope image                                        |
    | test_file_Overview_CIF.cif         | .cif   | Crystallographic Information File                                           |
    | README.md                          | .md    | ReadMe file providing information about the upload in MarkDown              |

Note that these files will not create entries in NOMAD, because a built-in parser for them doesn't exist. One exception is the `README.md` (Note the uppercase writing): This file will be used as preview on the upload page and can be used for explaining the content.

They will be stored in your upload and can be accessed and shared with your colleagues, however, they will not be searchable within NOMAD. In this case, NOMAD functions as a storage system, similar to a cloud drive.

You can add these files to your NOMAD upload. Do so by simply drag and drop the file or by opening the dialog to browse the files in your device.

??? task "Uploading files via drag and drop"
    **Drag and drop**

    Start with uploading all files. Let's use the drag and drop method as shown in the animation below.

    ![An animation demonstrating the drag and drop files in NOMAD ](images_crc1415/drag_and_drop_01.gif)

    When a compressed `.zip` file is uploaded to NOMAD, it will be extracted automatically and the included files will be added to your upload.

---

## Using NOMAD Oasis CRC1415 as ELN

Beside the cloud-drive storage, NOMAD Oasis CRC1415 can be used as electronic lab notebook (ELN) to enrich your data with useful metadata making it FAIR.
Furthermore the built-in [CRC1415-Plugin](https://github.com/CRC1415/NOMAD-CRC1415-Plugin) contains customized metadata fields and specific parsers for common experiments in the CRC1415.
To do so, perform these steps:

1. Click on button "CREATE FROM SCHEMA"
2. Provide a name for the sample entry.
3. Select the built-in schema "CRC1415-SampleOverview" and click "CREATE"
4. Provide as much information as possible about your sample.
5. When changing the form, save all changes by pressing the SAVE button!

**Use the arrow buttons ⬅️➡️ below to follow the steps for creating your first ELN entry.**

<div class="image-slider" id="sliderELN">
    <div class="nav-arrow left" id="prevELN">←</div>
    <img src="images_crc1415/schema_01.png" alt="Create a schema in uploads" class="active">
    <img src="images_crc1415/schema_02.png" alt="Create a new ELN entry by clicking on right button">
    <img src="images_crc1415/schema_03.png" alt="ELN overview page">
    <img src="images_crc1415/schema_04.png" alt="ELN overview page - save button">
    <div class="nav-arrow right" id="nextELN">→</div>
</div>


### Providing chemical information based on CIF

NOMAD has a built-in parser for `.cif` files and will automatically extract all relevant information:
- the unit cell 
- the chemical composition
- the chemical formula

These information are very useful for further searching features based on elements or symmetry groups.

**Use the arrow buttons ⬅️➡️ below to follow the steps for parsing your first CIF file.**

<div class="image-slider" id="sliderCIF">
    <div class="nav-arrow left" id="prevCIF">←</div>
    <img src="images_crc1415/schema_cif_01.png" alt="Select the CIF metadata in schema" class="active">
    <img src="images_crc1415/schema_cif_02.png" alt="Provide name of CIF upload and save">
    <img src="images_crc1415/schema_cif_03.png" alt="Chemical information summary">
    <img src="images_crc1415/schema_cif_04.png" alt="Select the overview page">
    <img src="images_crc1415/schema_cif_05.png" alt="Materials preview page">
    <div class="nav-arrow right" id="nextCIF">→</div>
</div>

### Parsing data and metadata based on files

The [NOMAD-CRC1415-Plugin](https://github.com/CRC1415/NOMAD-CRC1415-Plugin) has some custom file parser for the experiments in the CRC1415.
Usually you only need to select the measurement method and drag-and-drop the file to their metadata field.  
The parsing starts and extract as much information as possible out of the file. Additionally, in most cases a interactive plot will be generated.
On the OVERVIEW tab, all information of the sample will be presented.

**Use the arrow buttons ⬅️➡️ below to follow the steps for creating your first measurement entry.**

<div class="image-slider" id="sliderXRD">
    <div class="nav-arrow left" id="prevXRD">←</div>
    <img src="images_crc1415/schema_xrd_01.png" alt="Select the measurement in the schema" class="active">
    <img src="images_crc1415/schema_xrd_02.png" alt="Provide name of upload and save">
    <img src="images_crc1415/schema_xrd_03.png" alt="Plot and parsed information">
    <div class="nav-arrow right" id="nextXRD">→</div>
</div>

!!! info "If you need a custom parser for your measurement, please contact the [INF project of the CRC145](https://tu-dresden.de/mn/chemie/sfb1415/forschung/zentrale-projekte/projekt-inf)."

??? task "Parsing Raman file"
    **MeasurementRaman**

    Start with uploading the `.tvb` file. Follow the steps shown in the animation below.

    ![An animation demonstrating the parsing of Raman data in NOMAD ](images_crc1415/schema_raman_01.gif)

    After providing the `.tvb` file and saving the entry, the (meta)data extraction will automatically start.
    

??? task "Showing tif files"
    **MeasurementSEM/TEM**

    Start with uploading the `.tif` file. Follow the steps shown in the animation below.

    ![An animation demonstrating the parsing of Raman data in NOMAD ](images_crc1415/schema_sem_01.gif)

    At the moment, there's no built-in support for rendering `.tif` files in NOMAD. Instead please use this plugin in the meantime.
    

---

## Create datasets and get a DOI

You can organize several entries by grouping them into common datasets, making it easier to manage related data.
Datasets are for organizing and referencing curated data. They do not affect how data is processed.
Users can get a DOI for their datasets on [https://nomad-lab.eu](https://nomad-lab.eu).

!!! info "For creating datasets, please [read the tutorial](../tutorial/upload_publish.html#create-datasets-and-get-a-doi) about it."

!!! warning "At the moment, there's no DOI creation service within NOMAD Oasis CRC1415. Please use the worldwide accessible [NOMAD service](https://nomad-lab.eu) or the [CRC1415 Zenodo community](https://zenodo.org/communities/crc1415/about). If you need help, please contact the [INF project of the CRC145](https://tu-dresden.de/mn/chemie/sfb1415/forschung/zentrale-projekte/projekt-inf)."

---
