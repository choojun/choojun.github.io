## Secure Library: A digitize library with XXX algorithm 

### Setup:
1. Install Java Development Kit 1.8 from https://www.oracle.com/technetwork/java/javase/downloads/index.html
2. Install Eclipse IDE for Java EE Developers (Neon 3 RC3) from https://www.eclipse.org/downloads/packages/release/neon/3rc3/eclipse-ide-java-ee-developers
3. Install Apache Tomcat 7.x from http://tomcat.apache.org/download-70.cgi
4. Setup the Eclipse IDE with Apache Tomcat based on steps at https://www.eclipse.org/webtools/jst/components/ws/1.5/tutorials/InstallTomcat/InstallTomcat.html
5. Download the modified sample webapp of JDAL-Spring-MVC-DisplayTag from https://github.com/choojun/share/releases/tag/1.0.0.2 . Note that details of related library used in the webapp are available at http://www.jdal.org/doc/displaytag.php 
6. Import the eclipse project (securelibrary) with steps as follows.
   * Open Eclipse
   * Click File --> Import
   * Type Maven in the search box under Select an import source:
   * Select Existing Maven Projects
   * Click Next
   * Click Browse and select the folder that is the root of the Maven project (probably contains the pom.xml file)
   * Click Next
   * Click Finish
7. Run/Re-run the webapp (for each modification on source codes and configuration files) with steps as follows.
   * Right-click the project's name in Eclipse
   * Choose Run As --> Run on Server
   * Choose Tomcat 7.0 --> Next --> Finish
   * The webapp previewable at http://localhost:8080/securelibrary/

### To do:
1. Add additional column (e.g. pdf of book) for storing Blog data type in existing table. Suppose the created column will be used to store the encrypted file after user uploading. As such, modifications on exiting SQL scripts together with related JSP and JAVA files are required. Note that the database in-use, i.e. H2 database, supports the BLOB data type as stated at http://www.h2database.com/html/datatypes.html#blob_type
2. Enhance the current web-based GUI for handling i) file uploading and ii) record (BLOD of pdf) inserting into the created column at Step 1. More reading from http://www.jdal.org/doc/displaytag.php is required.
3. Identify the XXX algorithm. i) Locate the encryption of XXX algorithm on the uploaded file but before the process of inserting into database. ii) Locate the decryption of XXX algorithm on the encrypted file (e.g. pdf of book) but before the authenticated user previewing.
4. Modify related files (may involve some configurations too) of the webapp for user login. Suppose that both authenticated and unauthenticated users preview the same GUI of webapp. However, only authenticated user allows for previewing the encrypted file (e.g. pdf book). 

