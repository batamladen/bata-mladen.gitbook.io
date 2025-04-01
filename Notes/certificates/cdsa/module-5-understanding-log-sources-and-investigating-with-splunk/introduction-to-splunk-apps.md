# Introduction to Splunk Apps

## **What are Splunk Applications?**

* Splunk apps are packages that extend the capabilities of Splunk Enterprise or Splunk Cloud.
* They manage specific types of operational data and are tailored for different technologies and use cases.
* Apps act as pre-built knowledge packages, offering features such as:
  * Custom data inputs
  * Custom visualizations
  * Dashboards
  * Alerts
  * Reports

***

## **Benefits of Splunk Apps**

* Enable multiple workspaces within a single Splunk instance.
* Cater to different user roles and use cases.
* Available for download on **Splunkbase**.
* Many are designed for **Security Information and Event Management (SIEM)**, helping detect and respond to security threats.
* Facilitate data ingestion, analysis, and visualization for security investigations.

## **Considerations When Using Splunk Apps**

* **Data Volume & Hardware Requirements**: Some apps require significant system resources.
* **Licensing**: Premium apps may require additional licenses.
* **Increased License Usage**: Additional data inputs can lead to higher license consumption.

***

## **Sysmon App for Splunk - Installation & Usage**

**Developed by: Mike Haag**

**Steps to Install & Configure**

1. **Sign Up on Splunkbase**
   * Create a free account on **Splunkbase**.
2. **Log into Splunkbase**
   * Use the credentials to access available apps.
3. **Download Sysmon App for Splunk**
   * Navigate to the app page and download it.
4. **Add the Application to Your Search Head**
   * Install the app within your Splunk instance.
5. **Adjust the App’s Macros**
   * Ensure that events load correctly by modifying the app macros.
6. **Access the Sysmon App in Splunk**
   * Locate it under the **Apps** column on the Splunk home page.
7. **Navigate to the File Activity Tab**
   * Open the **File Activity** section within the app.
8. **Set Time Picker to "All Time" & Submit**
   * Adjust the time filter and submit the search.
9. **Fix Missing Results in "Top Systems" Section**
   * Click **Edit** (upper right corner) and modify the search query.
   * Replace the missing **Computer** field with **ComputerName**.
   * Click **Apply** to generate results successfully.

***

#### **Key Takeaways**

* Splunk Apps enhance data management and analysis capabilities.
* Splunkbase is the primary source for downloading and installing apps.
* The Sysmon App for Splunk helps monitor **system activities** but requires proper configuration.
* Understanding field names (e.g., **Computer** vs. **ComputerName**) is crucial for accurate data visualization.

***

## Tasks

### Task1

Access the Sysmon App for Splunk and go to the "Reports" tab. Fix the search associated with the "Net - net view" report and provide the complete executed command as your answer. Answer format: net view /Domain:\_.local

I didnt understand the question and how to even get to the answe, i found it on reddit.

```
`sysmon` process=net.exe (CommandLine="net view /DOMAIN:uniwaldo.local") | stats count by Computer,CommandLine
```

**Answr: net view /DOMAIN:uniwaldo.local**

***

### Task2

Access the Sysmon App for Splunk, go to the "Network Activity" tab, and choose "Network Connections". Fix the search and provide the number of connections that SharpHound.exe has initiated as your answer.

Edit the search to this: Use a wildcard for the image that created the event, open the search that gets outputed and count the number of occurances.

```
sysmon EventCode=3 Image="*SharpHound.exe" | eval Destination=coalesce(dest_host,dest_ip) | stats count, values(Destination) AS "Destinations", values(dest_port) AS "Ports", values(protocol) AS "Protocols" by Image | fields Image Destinations Ports Protocols count
```

**Answer: 6**
