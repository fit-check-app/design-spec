# Data architecture overview

## Database choice

The database choice for this project will be a SQL database. A tried and true technology, SQL databases will provide the necessary data structuring and querying capabilities for this project. The RDBMS of choice will be MySQL. As a popular open-source database, MySQL will most likely have the necessary driver packages needed to interact with the database form the chosen backend language.

## Blob storage choices

Since the project will be dealing with images, a blob storage solution will be needed. The choice of blob storage will be [MinIO](https://min.io). This is a great choice because it is an open-source cloud native object storage server. Since we may be looking at utilizing Kubernetes for the project, MinIO will slip right into the container orchestration system with ease. With its support for S3, it will be much easier to migrate to a cloud provider if the need arises.

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
