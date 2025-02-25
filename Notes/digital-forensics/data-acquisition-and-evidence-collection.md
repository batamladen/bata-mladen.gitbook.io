# Data Acquisition & Evidence Collection

## Understand Data Acquisition Fundamentals

To perform a forensic examination on a potential source of evidence, the first step is to create a replica of the data residing on the media found in the crime scene such as a hard disk or any other digital storage device. Forensic investigators can either perform the data acquisition process on-site, or first transport the device to a safe location.

***

### Data Acquisition

Data acquisition is the use of established methods to extract <mark style="color:yellow;">Electronically Stored Information (ESI)</mark> from suspect computer or storage media to gain insight into a crime.

Investigators must be able to verify the accuracy of acquired data, and <mark style="color:yellow;">the complete process should be auditable and accepted in the court</mark>.

There are 2 types of data acquisition:

* Live Acquisition - Involves collecting data from a system that is powered **ON**
* Dead Acquisition (static) - Involves collecting data from a system that is powered OFF

***

#### Live Acquisition

The live data acquisition process involves the collection of volatile data from devices when they are live or powered on.

Volatile information assists in determining the **logic timeline** of the security incident, and the **possible users responsible**

Live acquisition can then be followed by static/dead acquisition, where the investigator shuts down the suspect machine, removes the hard disk, and then acquires its forensic image.

Types of data captured during live acquisition:

1. System data
   * Logged on users
   * Data and time
   * Current configuration
   * Temp and swap files
2. Network data
   * Routing tables
   * ARP Cache
   * Network configuration
   * Network connections

***

#### Order of Volatility

When collecting evidence, and investigator needs to evaluate the order of volatility of data depending on the suspect machine and situation.

According to RFC 3227, this is the order of volatility:

1. Registers and cache
2. Routing tables, process table, kernel statistics, memory
3. Temp system files
4. Disk or other storage media
5. Remote logging
6. Physical config and network topology
7. Archival media

***

### Dead Acquisition

Dead acquisition is defined as the acquisition from a suspect machine that is powered off. Dead acquisition ussualy involes acquiring data form storage devices (hard drives, DVD-ROMs, USB dives...)

**Example of static data**: Mails, Word Docs, Drive Space, Deleted Files.

***

### Rule of Thumb for Data Acquisition

A rule of thumb is a best practice that helps to ensure a favorable outcome when applied.

* Do not work on original digital evidence. Create a bit-stream image to work on.
* Produce two or more copies of original media.
  * First is the working copy
  * Second acts like a library, for disclosure purposes
* Use clean medai to store copies
* Upon creating copies of original media, verify the integrity

***

***

## Different Types of Data Acquisition

There are:

### Logical Acquisition

In a situation with time constraints and where the investigator is aware of what files need to be acquired, logical acquisition may be considered ideal. Logical acquisition gathers only the files required for the case investigation. For example: o Collection of Outlook .pst or .ost files in email investigations o Specific record collection from a large RAID server

### Sparse Acquisition

Sparse acquisition is similar to logical acquisition. Through this method, investigators can collect fragments of unallocated (deleted) data. This method is useful when it is not necessary to inspect the entire drive.

***

### Bit steam Image

A bit-stream image is a bit-by-bit copy of any storage media that contains a cloned copy of the entire media, including all its sectors and clusters. This cloned copy of the storage media contains all the latent data that enables investigators to retrieve deleted files and folders. Investigators often use bit-stream images of the suspect media to prevent contamination of the original media.

forensic tools such as **FTK Imager** and **EnCase**, can read bit-stream images.

**There are two kinds of bit-stream imaging procedures:**

#### Bit-stream disk-to-image-file

Forensic investigators commonly use this data acquisition method. It is a flexible method that enables the creation of one or more copies of the suspect drive.

Tools such as ProDiscover, EnCase, FTK can be used to create image files.

#### Bit-stream disk-to-disk

Investigators cannot create a bit-stream disk-to-image file in the following situations: o The suspect drive is very old and incompatible with the imaging software o There is a need to recover credentials used for websites and user accounts

In such cases, a bit-stream disk-to-disk copy of the original disk or drive can be performed. While creating a disk-to-disk copy, the geometry of the target disk, including its head, cylinder, and track configuration, can be modified to align with the suspect drive.

***

***

## Determine the Data Acquisition Format

### Raw Images

Raw format creates a bit-by-bit copy of the suspect drive. Images in this format are usually obtained by using the dd command.

➢Advantages:

* Fast data transfers
* Minor data read errors on source drive are ignored Read by most of the forensic tools

➢ Disadvantages

* Requires same amount of storage as that of the original media
* Tools (mostly open source) might fail to recognize/collect marginal (bad) sectors from the suspect drive

***

### Advanced Forensics Format (AFF)

AFF is an open-source data acquisition format that stores disk images and related metadata.

* No size limitation for disk-to-image files
* Option to compress file
* Simple design
* Allocates space to record metadata for image

File extensions iclude **.afm** for AFF metadata and **.afd** for segmented image.

***

### Advanced Forensic Framework 4 (AFF4)

***

***

## Understand Data Acquisition Methodology

Forensic investigators must adopt a systematic and forensically sound approach while acquiring data from suspect media. This is to increase the chances that the evidence is admissible in the court of law.



<figure><img src="../.gitbook/assets/Pasted image 20241211124404.png" alt=""><figcaption></figcaption></figure>

While performing forensic data acquisition, potential approaches must be carefully considered, and methodologies aimed at protecting the integrity and accuracy of the original evidence must be followed. Data acquisition must be performed as per departmental or organizational policies and in compliance with applicable standards, rules, and laws.

**Step 1: Determine the Best Data Acquisition Method** Based on size of suspect drive Time required to acquire the image If the drive can be retained

**Step 2: Select the Data Acquisition Tool** Imaging tools must be validated and tested to ensure that they produce accurate and repeatable results. These tools must satisfy certain requirements, some of which are mandatory (features and tasks that the tool must possess or perform), while some are optional.

**Step 3: Sanitize the Target Media** Before data acquisition and duplication, an appropriate data sanitization method must be used to permanently erase any previous information stored on the target media. Destruction of data using industry standard data destruction methods is essential for sensitive data that one does not want falling into the wrong hands.

**Step 4: Acquire Volatile Data** As the contents of RAM and other volatile data are dynamic, investigators need to be careful while acquiring such data. Working on a live system may alter the contents of the RAM or processes running on the system.

**Step 5: Enable Write Protection on the Evidence Media** Write protection refers to one or more measures that prevent a storage media from being written to or modified. It may either be implemented by a hardware device, or a software program on the computer accessing the storage media. Enabling write protection allows the data to be read but prohibits writing or modification.

**Step 6: Acquire Non-volatile Data** Non-volatile data can be acquired from a hard disk both during live and dead acquisition processes. Investigators can use remote acquisition tools such as Netcat, or bootable CDs or USBs via tools such as CAINE to perform live acquisition of a hard disk.

**Step 7: Plan for Contingency** In digital forensics investigation, planning for contingency refers to a backup program that an investigator must have in case certain hardware or software do not work, or a failure occurs during an acquisition. Contingency planning is necessary for all cyber investigations as it assists investigators in preparing for unexpected events.
