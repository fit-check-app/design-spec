# Data architecture overview

## Database choices

### SQL

- MySQL: MySQL is open source and has a free Community Edition, which is widely used and suitable for many applications.
- PostgreSQL: PostgreSQL is open source and released under a permissive open-source license. It is entirely free and often used for various applications.
- SQLite: SQLite is in the public domain, and its source code is freely available. It is free for both personal and commercial use and often embedded in applications.
- MariaDB: MariaDB, being an open-source fork of MySQL, is also free and open source. It offers a community version without licensing fees.

### NoSQL

- MongoDB:
    - MongoDB is a widely used open-source NoSQL database known for its flexibility and scalability.
    - It is a document-oriented database that stores data in BSON (Binary JSON) format.
    - MongoDB Community Edition is free and open source, and it's suitable for many applications.
- Cassandra:
    - Apache Cassandra is a distributed NoSQL database designed for high availability and scalability.
    - It is particularly well-suited for managing large amounts of data across multiple data centers.
    - Cassandra is open source and free to use.
- Redis:
    - Redis is an open-source, in-memory key-value store often used for caching and real-time data processing.
    - It supports a variety of data structures and is known for its high performance.
    - Redis is available under the BSD license.

## Blob storage choices

- GitHub
    - GitHub allows you to store code and data files in repositories. While primarily designed for version control, you can use GitHub to host and share files.
    - May need to consult terms & conditions to verify proper use and inspect for setup difficulty
- OpenStack Swift:
    - OpenStack Swift is open-source object storage software. You can set up your own Swift instance or use a provider that offers Swift-based storage.
    - Alleviates overhead of object storage 
- Local directory
    - Have the webservice simply save to a local directory
    - In a containerized environment, mounts to volumes can help with ease of deployment and backups

## Security concerns

- File Type Validation: Verify that the uploaded file's format matches the expected file type. Do not rely solely on the file extension; use content-type and magic bytes (file signatures) checks.
- File Size Limits: Set file size limits to prevent the upload of excessively large files that could exhaust server resources or lead to denial-of-service (DoS) attacks.
- Malicious Payloads: Scan uploaded files for malware, viruses, or other malicious payloads using antivirus software or third-party scanning services.
- File Overwriting: Ensure that uploaded files do not overwrite existing files with the same name. Use unique filenames or folders to prevent accidental or intentional overwriting.
- Path Traversal (Directory Traversal): Implement controls to prevent path traversal attacks. Ensure that users cannot access files outside the designated upload directory.
- Content Disposition: Set the Content-Disposition header to specify whether a file should be displayed in the browser or treated as a download to prevent content rendering vulnerabilities.
- Authentication and Authorization: Ensure that only authorized users can upload files. Authenticate and authorize users before allowing file uploads.
- Cross-Site Request Forgery (CSRF): Implement anti-CSRF measures to prevent attackers from tricking authenticated users into performing file uploads without their consent.
- Cross-Site Scripting (XSS): Sanitize and validate file metadata (e.g., file names) to prevent XSS attacks, as attackers can upload files with malicious filenames.
- Server Resources and Denial of Service: Limit concurrent file uploads to prevent resource exhaustion attacks, such as uploading a large number of files simultaneously.
- Privacy and Data Leakage: Be cautious with user-uploaded content that could contain sensitive information. Ensure that the data does not expose private or confidential data.
- Content Validation: Validate the content of uploaded files to prevent them from containing harmful scripts or executable code.
- Secure Storage: Store uploaded files in a secure location with restricted access. Consider encryption at rest to protect sensitive data.
- Logging and Monitoring: Implement logging and monitoring to track file uploads, detect suspicious activities, and respond to security incidents promptly.
- Server-Side Validation: Always perform server-side validation and processing of uploaded files to avoid relying on client-side security mechanisms.
- Access Control: Implement proper access control mechanisms to ensure that only authorized users can access the uploaded files.
- Data Retention Policies: Define data retention policies to manage and remove uploaded files that are no longer needed to reduce security risks.
