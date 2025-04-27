# PHP Advanced File Upload Techniques Cheatsheet

Welcome to the **PHP Advanced File Upload Techniques Cheatsheet** for **StackMagePHP** viewers! 🚀 This guide provides 10 advanced techniques to make your PHP file upload system secure, efficient, and user-friendly. Use this as a quick reference to enhance your web applications.

## Table of Contents
1. [Sanitize File Names](#1-sanitize-file-names)
2. [Restrict File Types with MIME Validation](#2-restrict-file-types-with-mime-validation)
3. [Limit File Upload Size Server-Side](#3-limit-file-upload-size-server-side)
4. [Generate Unique File Names](#4-generate-unique-file-names)
5. [Validate File Content (Image-Specific)](#5-validate-file-content-image-specific)
6. [Secure Upload Directory](#6-secure-upload-directory)
7. [Implement Chunked Uploads for Large Files](#7-implement-chunked-uploads-for-large-files)
8. [Use CSRF Protection](#8-use-csrf-protection)
9. [Log Upload Activities](#9-log-upload-activities)
10. [Validate File Extensions Securely](#10-validate-file-extensions-securely)

## 1. Sanitize File Names
Prevent malicious file names by sanitizing and renaming uploaded files.
```php
$safeFileName = preg_replace("/[^A-Za-z0-9._-]/", "", basename($_FILES["file"]["name"]));
$target_file = $target_dir . time() . "_" . $safeFileName;
```

## 2. Restrict File Types with MIME Validation
Use MIME types for stricter file validation with `finfo` for accuracy.
```php
$allowed_mimes = ["image/jpeg", "image/png", "application/pdf"];
$finfo = finfo_open(FILEINFO_MIME_TYPE);
$mime_type = finfo_file($finfo, $_FILES["file"]["tmp_name"]);
finfo_close($finfo);
if (!in_array($mime_type, $allowed_mimes)) {
    die("Invalid file type.");
}
```

## 3. Limit File Upload Size Server-Side
Enforce file size limits in PHP alongside HTML form restrictions.
```php
$max_size = 5 * 1024 * 1024; // 5MB
if ($_FILES["file"]["size"] > $max_size) {
    die("File too large. Max size: 5MB.");
}
```

## 4. Generate Unique File Names
Avoid overwrites by appending timestamps or UUIDs to file names.
```php
$unique_name = uniqid() . "_" . basename($_FILES["file"]["name"]);
$target_file = $target_dir . $unique_name;
```

## 5. Validate File Content (Image-Specific)
Check if an uploaded image is valid using `getimagesize`.
```php
if (!getimagesize($_FILES["file"]["tmp_name"])) {
    die("File is not a valid image.");
}
```

## 6. Secure Upload Directory
Prevent direct access to the upload directory using `.htaccess`.
```apache
# Place in uploads/.htaccess
Deny from all
```

## 7. Implement Chunked Uploads for Large Files
Handle large files by splitting them into chunks (requires JavaScript + PHP).
```php
// Example: Append chunk to file
$file_path = $target_dir . $_POST["file_name"];
file_put_contents($file_path, file_get_contents($_FILES["chunk"]["tmp_name"]), FILE_APPEND);
```

## 8. Use CSRF Protection
Protect file upload forms with CSRF tokens to prevent unauthorized submissions.
```php
session_start();
$token = bin2hex(random_bytes(32));
$_SESSION["csrf_token"] = $token;
```
```html
<input type="hidden" name="csrf_token" value="<?php echo $token; ?>">
```
```php
if ($_POST["csrf_token"] !== $_SESSION["csrf_token"]) {
    die("Invalid CSRF token.");
}
```

## 9. Log Upload Activities
Track uploads for debugging or auditing purposes.
```php
$log_message = date("Y-m-d H:i:s") . " - Uploaded: " . $_FILES["file"]["name"] . " by " . $_SERVER["REMOTE_ADDR"] . "\n";
file_put_contents("upload_log.txt", $log_message, FILE_APPEND);
```

## 10. Validate File Extensions Securely
Combine extension checks with MIME types for robust validation.
```php
$allowed_extensions = ["jpg", "png", "pdf"];
$file_ext = strtolower(pathinfo($_FILES["file"]["name"], PATHINFO_EXTENSION));
if (!in_array($file_ext, $allowed_extensions)) {
    die("Invalid file extension.");
}
```

## Pro Tips
- **Test in a Secure Environment**: Always test uploads in a sandboxed environment to avoid security risks.
- **Backup Uploads**: Regularly back up your `uploads/` directory to prevent data loss.
- **Combine Techniques**: Use multiple techniques (e.g., MIME validation + CSRF) for a robust system.

## Learn More
📺 **Want to see these in action?** Watch our **PHP File Upload** tutorial on **StackMagePHP**!  
👉 **Like, Share, and Subscribe** for more PHP tips! 🚀  
#PHPFileUpload #LearnPHP #WebDevelopment #StackMagePHP

## Contributing
Found a better technique or want to improve this cheatsheet? Share your feedback with **StackMagePHP** https://www.youtube.com/channel/UC4wsv4V_HJ30Fz69mUwY52g on YouTube or contribute via our community!

## License
This cheatsheet is provided for educational purposes by **StackMagePHP**. Use it freely in your projects, but please credit **StackMagePHP** when sharing.
