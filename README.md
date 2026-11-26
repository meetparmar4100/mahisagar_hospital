# Mahisagar Hospital — Hostinger PHP + MySQL edition
This separate package preserves the public website and compact CMS from commit d3b905bc5122075f7b071be941ef9ae0cd9f5870. No Cloudflare Worker, D1, KV or credentials required. Initial website content is the published snapshot (scroll duration 700 ms). Draft-only changes, old login sessions and passwords are not migrated.
## Deploy in Hostinger hPanel
1. Choose a PHP/MySQL web hosting website, PHP 8.2 or newer; enable SSL/HTTPS. This is not a static-only website builder deployment. Use a dedicated domain/document root.
2. Back up existing public_html. Extract the ZIP on your computer. Upload the **contents of the extracted repository folder** directly into public_html, including .htaccess and .user.ini; do not upload the enclosing repository folder as a subdirectory.
3. hPanel > Databases > Management: create a MySQL database and user. Copy the exact prefixed database and username. In File Manager edit private/config.php: db_host (usually localhost), db_name, db_user, db_pass. Set setup_key to a unique random string of at least 24 characters. Never commit real credentials.
4. Open https://YOUR-DOMAIN/setup using HTTPS. Enter the installation key and choose an admin password (16–64 characters). The installer initializes MySQL and creates admin. It locks after installation. If private/uploads is not writable, correct hosting permissions (typically 0750/0755 with PHP user ownership; never 0777).
5. Open https://YOUR-DOMAIN/admin. Login ID: admin; password chosen at setup. Publish a harmless test edit and verify the public website. Check image/video upload and logout.
6. Verify direct requests to /private/config.php and /private/seed.json return 403 (or 404), and unauthenticated /api/admin/data returns 401. Do not use this package on a server that ignores .htaccess access rules. Keep HTTPS enabled.
## Storage
MySQL: cms_content, cms_admin, cms_attempts, cms_media. Local files: private/uploads. Bundled images: public/assets. Media uploaded through admin is served by /media/{id}, not executable file paths. Removing an image from content does not erase its stored file. Back up the MySQL database and private/uploads regularly.
## Requirements
PHP 8.2+, PDO MySQL, fileinfo, Apache/LiteSpeed-compatible .htaccess and mod_rewrite. Upload max 20 MiB. No Cloudflare binding, npm build or external storage. External HTTPS links entered by the admin remain external by design.
