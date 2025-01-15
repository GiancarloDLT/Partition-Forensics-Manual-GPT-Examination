<h1>Partition Forensics: Manual GPT Examination</h1>


<h2>Description</h2>
<br>This project demonstrates a detailed analysis of GUID Partition Tables (GPT) and Master Boot Records (MBR) using hex dumps. It includes manual interpretation of offsets and sector data to identify key partition metadata such as type codes, sector boundaries, partition sizes, and attributes. Through a detailed examination of the hex values, I identified key details about the disk's structure and storage allocation, offering insights into partition organization and behavior.

The significance of this project lies in its application to digital forensics and data recovery. Understanding the structure of partition tables is crucial for analyzing storage devices, recovering lost partitions, and uncovering potential tampering or corruption. By dissecting the disk's low-level layout, this project helps illustrate the practical relevance of partition analysis in maintaining data integrity and investigating disk-related incidents.<br />


<h2>Languages and Utilities Used</h2>

- <b>FTK</b>
- [MBR Partition Table](https://i.imgur.com/0p7z4SE.png)
- [GPT Partition Table Scheme](https://i.imgur.com/GltHJXc.png)
- [Partition Entries Offset](https://i.imgur.com/GltHJXc.png)



<h2>Environments Used </h2>

- <b>Windows 10</b>

<h2>Walk-through:</h2>
<br>
<br>
<p align="left">
<b> For the Master Boot Record (MBR) analysis, I reference the MBR Partition Table provided in the repository, along with the scheme outlined below:
  The entries are as follows:
  
- Byte 01: Boot Indication (00h – not bootable, 80h – bootable)
- Bytes 02-04: Head (starting number), Sector (starting number), Cylinder (starting number)
- Byte 05: System Indicator (Partition Type)
- Bytes 06-08: Head (ending number), Sector (ending number), Cylinder (ending number)
- Bytes 09-12: Number of sectors before the partition (Relative Sectors)
- Bytes 13-16: Number of sectors in the partition (Length)
  <b/>
<br>
<br>

<b> Evidence Analysis – LBA 0

The first LBA sector (LBA 0) shows the presence of the Protected Master Boot Record (PMBR), which ensures backward compatibility with systems using Master Boot Record (MBR) partitioning. The following details summarize the information extracted from LBA 0:

- Boot Flag: Non-bootable
- CHS Beginning: Head 0, Sector 2, Cylinder 0
- Type Code: EE
- CHS End: Head 1023, Sector 63, Cylinder 255
- LBA Beginning: 1
- Number of Sectors: 4,294,967,295 sectors (2.2 TB).<b/>
  <img src="https://i.imgur.com/zmFjsM6.png" height="80%" width="80%" alt="Network Segmentation Basics VLANs and Inter VLAN Communication Steps"/>
<br>
<br>

<b> Evidence Analysis – LBA 1-33

The next analysis begins with the Primary GPT Header, located at LBA 1, which contains crucial metadata for the GUID Partition Table.

Key Attributes from the GPT Header:

- Signature (EFI Part): 45 46 49 20 50 41 52 54 (identifies this as a GPT disk).
- GPT Version: 2.3.1 (00 00 01 00)
- Header Size: 92 bytes (Little Endian: 00 00 00 5C)
- CRC32 Checksum of Header: 7A A6 58 B9
- Current LBA Sector: 01
- Backup Header LBA: 7,999,999
- First Partition LBA: 34
- Last Usable LBA: 7,999,966
- Disk GUID: 66571E79-EC68-49B0-87B6-B1D23B5FA528
- Partition Table Start LBA: 2
- Total Partitions: 128 (128 entry spaces in the partition table)
- Size of Partition Entry: 128 bytes each
- CRC32 Checksum of Partition Array: 3C76704D.<b/>
  <img src="https://i.imgur.com/o5sIC3N.png" height="80%" width="80%" alt="Network Segmentation Basics VLANs and Inter VLAN Communication Steps"/>
<br>
<br>

<b> Evidence Analysis – LBA 2 - 34

The GPT partition entries begin at LBA 34, with the first partition metadata located there.

Partition 1:

- Partition Type GUID: E3C0E316-0B5C-4DB8-817D-F92DF00215AE
- Unique Partition GUID: 5A61B6B6-BF7F-4DB4-8ACE-995FE7F6C927
- Starting Sector: LBA 34 (0x22)
- Ending Sector: LBA 4113 (0x10020)
- Partition Size: 2.09 MB (4113 sectors × 512 bytes)
- Attributes: Microsoft reserved partition<b/>
  <img src="https://i.imgur.com/7kWjHjZ.png" height="80%" width="80%" alt="Network Segmentation Basics VLANs and Inter VLAN Communication Steps"/>
<br>
<br>

<b> Partition 2:

- Partition Type GUID: EBD0A0A2-B9E5-4433-87C0-68B6B72699C7
- Unique Partition GUID: 7B39EEBA-F45E-44A7-AA68-6E0D8FEEACCA
- Starting Sector: LBA 1048704 (0x10080)
- Ending Sector: LBA 475263 (0x7407F)
- Partition Size: ~293.6 MB
- Attributes: Microsoft reserved partition<b/>
  <img src="https://i.imgur.com/v5JhrXq.png" height="80%" width="80%" alt="Network Segmentation Basics VLANs and Inter VLAN Communication Steps"/>
<br>
<br>

<b> Partition 3:

- Partition Type GUID: EBD0A0A2-B9E5-4433-87C0-68B6B72699C7
- Unique Partition GUID: 9F1D5A70-D851-4915-81ED-C9CFBD43EB
- Starting Sector: LBA 475264 (0x74080)
- Ending Sector: LBA 893055 (0xDA07F)
- Partition Size: ~214 MB
- Attributes: Microsoft reserved partition<b/>
  <img src="https://i.imgur.com/k3aoRDH.png" height="80%" width="80%" alt="Network Segmentation Basics VLANs and Inter VLAN Communication Steps"/>
<br>
<br>

<b> Partition 4:

- Partition Type GUID: EBD0A0A2-B9E5-4433-87C0-68B6B72699C7
- Unique Partition GUID: EADB7D12-B094-47C2-A0C1-99066CA66B90
- Starting Sector: LBA 893056 (0xDA080)
- Ending Sector: LBA 1337471 (0x14687F)
- Partition Size: ~228 MB
- Attributes: Microsoft reserved partition<b/>
  <img src="https://i.imgur.com/viYFGII.png" height="80%" width="80%" alt="Network Segmentation Basics VLANs and Inter VLAN Communication Steps"/>
<br>
<br>

<b> Partition 5:

- Partition Type GUID: EBD0A0A2-B9E5-4433-87C0-68B6B72699C7
- Unique Partition GUID: 66CCFB1E-11E9-3211-A9B7-4C9B84F8186A
- Starting Sector: LBA 1337472 (0x146880)
- Ending Sector: LBA 2054271 (0x1F587F)
- Partition Size: ~367 MB
- Attributes: Microsoft reserved partition<b/>
  <img src="https://i.imgur.com/xm8qGpg.png" height="80%" width="80%" alt="Network Segmentation Basics VLANs and Inter VLAN Communication Steps"/>
<br>
<br>

<b> There are a total of seven partitions analyzed in this project. For those interested in further exploration, I encourage you to dive into the analysis, as it presents an accessible challenge for those willing to take it on. The complete GPT_Analysis.001 RAR file will be available in the repository for download..<b/>
<br>
<br>












