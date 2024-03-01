# Setting Up Nginx on Your EC2 Instance

Follow these steps to set up Nginx on your EC2 instance.

1. **Connect to Your EC2 Instance**: Use SSH to connect to your EC2 instance.

2. **Update Your System**: Run the following commands to update your package repository and installed packages:

    ```bash
    sudo apt update
    sudo apt upgrade
    ```

3. **Install Nginx**: Use the following command to install Nginx:

    ```bash
    sudo apt install nginx
    ```

4. **Start Nginx**: Once Nginx is installed, it should start automatically. If it doesn't, you can start it using:

    ```bash
    sudo systemctl start nginx
    ```

5. **Enable Nginx to Start on Boot** (Optional): To ensure Nginx starts automatically when the server restarts, run:

    ```bash
    sudo systemctl enable nginx
    ```

6. **Navigate to Nginx's Configuration Directory**: The main Nginx configuration file is typically located in `/etc/nginx/nginx.conf`, but for server blocks, you'd usually modify the configuration in `/etc/nginx/sites-available/`. You'll also need to create a symbolic link to enable your site. Let's use the default server block for simplicity:

    ```bash
    cd /etc/nginx/sites-available/
    ```

7. **Edit the Default Server Block Configuration**: Open the default server block configuration file using a text editor:

    ```bash
    sudo vim default
    ```

8. **Configure Nginx to Serve Your index.html File**: Inside the server block, ensure you have a section like this:

    ```nginx
    server {
        listen 80 default_server;
        listen [::]:80 default_server;

        root /var/www/html;
        index index.html;

        server_name _;

        location / {
            try_files $uri $uri/ =404;
        }
    }
    ```

    Make sure the `root` directive points to the directory where your `index.html` file is located. In this case, it's set to `/var/www/html`, which is Nginx's default document root directory.

9. **Save and Close the File**: After making the necessary changes, save the file and exit the text editor.

10. **Test Nginx Configuration**: Before restarting Nginx, it's a good idea to test the configuration to catch any syntax errors:

    ```bash
    sudo nginx -t
    ```

11. **Reload Nginx Configuration**: If the test is successful, reload Nginx to apply the changes:

    ```bash
    sudo systemctl reload nginx
    ```

12. **Access Your Website**: You should now be able to access your website by entering your EC2 instance's public IP address or domain name into a web browser.
