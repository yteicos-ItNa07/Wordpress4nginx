# Wordpress4nginxCertainly! Below is the GitHub README content for your WordPress Nginx configuration kit. You can include this in your repository's README.md file:

```markdown
# WordPress Nginx Configuration Kit

This config kit contains the Nginx configurations used in the "Install WordPress on Ubuntu 22.04" guide. It incorporates best practices from various sources, including the WordPress Codex and H5BP. The following example sites are included:

- `multisite-subdirectory.com`: WordPress multisite install using subdirectories
- `multisite-subdomain.com`: WordPress multisite install using subdomains
- `single-site.com`: WordPress single site install
- `single-site-with-caching.com`: WordPress single site install with FastCGI caching
- `single-site-no-ssl.com`: WordPress single site install (no SSL or page caching)

## Usage

### Site Configuration

You can use these sample configurations as a reference or directly replace your existing Nginx directory. Follow these steps to replace your existing Nginx configuration:

1. **Backup any existing config**:
   ```
   sudo mv /etc/nginx /etc/nginx.backup
   ```

2. **Copy these configs to `/etc/nginx`**.

3. **Symlink the default file from `sites-available` to `sites-enabled`**:
   ```
   sudo ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/default
   ```

4. **Copy one of the example configurations from `sites-available` to `sites-available/yourdomain.com`**:
   ```
   sudo cp /etc/nginx/sites-available/single-site.com /etc/nginx/sites-available/yourdomain.com
   ```

5. **Edit the site accordingly**, paying close attention to the server name and paths.

6. **To enable the site, symlink the configuration into the `sites-enabled` directory**:
   ```
   sudo ln -s /etc/nginx/sites-available/yourdomain.com /etc/nginx/sites-enabled/yourdomain.com
   ```

7. **Test the configuration**:
   ```
   sudo nginx -t
   ```

8. **If the configuration passes, restart Nginx**:
   ```
   sudo service nginx reload
   ```

### PHP Configuration

The php-fpm pool configuration is located in `global/php-pool.conf` and defaults to PHP 7.4. Modify it if you want the default php-fpm pool service to be a different PHP version. Additional PHP version upstream definitions can be added to the `/upstreams` folder (a PHP 7.3 sample is provided there). You can either use the default pool using `$upstream` in your Nginx configurations or the specific upstream definition (e.g., `php73`, `php72`) set up by your custom upstream definitions.

For example, currently, the Nginx configuration for `single-site.com` has the following set for PHP requests:

```
fastcgi_pass    $upstream
```

You could change that to the following to use the PHP 7.3 service instead (assuming that `php7.3-fpm` service is running):

```
fastcgi_pass    php73
```

This allows different server blocks to execute different PHP versions if needed.

### Directory Structure

This config kit follows the conventions used by a default Nginx install on Debian:

- `conf.d`: Configurations for additional modules.
- `global`: Configurations within the `http` block.
  - `global/server`: Configurations within the `server` block. The `defaults.conf` file contains sensible defaults for caching, file exclusions, and security. Additional `.conf` files can be included as needed on a per-site basis.
- `sites-available`: Configurations for individual sites (virtual hosts).
- `sites-enabled`: Symlinks to configurations within the `sites-available` directory. Only sites that have been symlinked are loaded.

### Recommended Site Structure

The following site structure is used throughout these configs:

```
.
├── yourdomain1.com
    ├── cache
    ├── logs
    └── public
├── yourdomain2.com
    ├── cache
    ├── logs
    └── public
```

