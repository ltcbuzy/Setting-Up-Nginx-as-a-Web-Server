# Setting Up Nginx as a Web Server and Reverse Proxy for Apache on Ubuntu 18.04 Server

![Setting Up](https://github.com/ltcbuzy/Setting-Up-Nginx-as-a-Web-Server-/assets/96268218/3f949ed6-d24c-4fd6-a1e5-09f752076096)


Configuring Nginx as a web server and reverse proxy for Apache on Ubuntu 18.04 is a strategic approach to optimizing web hosting performance. By leveraging Nginx's efficient handling of incoming requests and its seamless integration with Apache, this setup enhances overall server scalability, performance, and security.

To embark on this configuration, start by installing Nginx and Apache on your Ubuntu server. Nginx will act as the primary web server, managing incoming connections, while Apache handles the backend processing.

Adjust the Apache configuration to listen on a specific port, different from Nginx's default port. This ensures a clear separation of responsibilities between the two servers. Modify the default Apache virtual host configuration to reflect the new port, such as 8080.

Nginx will then be configured as a reverse proxy, directing incoming requests to Apache. This is achieved by updating Nginx's default configuration file. Ensure the **proxy_pass** directive points to Apache's listening address and port, maintaining the specified port from the Apache configuration.

After making these adjustments, test the Nginx configuration for any errors. If the test is successful, reload Nginx to apply the changes.

Additionally, if you're using a firewall tool like UFW, ensure that the necessary ports, such as 80 and 8080, are allowed to facilitate smooth traffic flow.

To verify the successful configuration, visit your domain or server IP in a web browser. Nginx should act as a reverse proxy, efficiently forwarding requests to Apache.

For any specific guidance or assistance in resolving issues during this configuration process, please don't hesitate to contact us. Our team is ready to provide support, ensuring your Nginx and Apache integration is seamless and optimized for your unique hosting requirements.

## Step 1: Install Nginx

Update the package list and install Nginx:
```
sudo apt update
sudo apt install nginx
```
## Step 2: Install Apache

Install Apache and enable it to start on boot:
```
sudo apt install apache2
sudo systemctl enable apache2
```
## Step 3: Configure Apache

Adjust the Apache configuration to listen on a different port. Open the Apache default configuration file:
```
sudo nano /etc/apache2/ports.conf
```
Change the Listen directive to a different port, for example, 8080:
```
Listen 8080
```
Save and close the file. Then, edit the default Apache virtual host configuration:
```
sudo nano /etc/apache2/sites-available/000-default.conf

```
Change the **<VirtualHost>** block to reflect the new port:
```
<VirtualHost *:8080>
    ...
</VirtualHost>

```
Save and close the file. Restart Apache for the changes to take effect:
```
sudo systemctl restart apache2
```
## Step 4: Configure Nginx as a Reverse Proxy

Edit the Nginx default configuration:
```
sudo nano /etc/nginx/sites-available/default

```
Replace the contents with the following configuration. Adjust the server_name and proxy_pass directives as needed:
```
server {
    listen 80;
    server_name example.com www.example.com;

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # Additional Nginx configurations can be added here
}
```
Save and close the file. Test the Nginx configuration:

```
sudo nginx -t
```
If there are no errors, reload Nginx:
```
sudo systemctl reload nginx
```
## Step 5: Adjust Firewall Settings

If you are using UFW, allow traffic on the configured ports:
```
sudo ufw allow 80
sudo ufw allow 8080
```
## Step 6: Test the Configuration

Visit your domain or server IP in a web browser. Nginx should act as a reverse proxy, forwarding requests to Apache.

This setup allows Nginx to handle incoming requests and pass them to Apache for processing, combining the strengths of both web servers. Adjust the configuration based on your specific requirements and domain settings.

Deploying Nginx as a web server and reverse proxy for Apache on Ubuntu 18.04 streamlines the web hosting process, leveraging the strengths of both servers. Nginx effectively manages incoming requests and seamlessly directs them to Apache, resulting in improved performance, scalability, and security. The setup entails installing Nginx, fine-tuning Apache configurations, and configuring Nginx as a reverse proxy. If you encounter any issues or need specific guidance, feel free to [reach out to us](https://t.ly/rUUG5). Our team is committed to assisting you in achieving a smooth integration of Nginx and Apache, ensuring an optimized web server setup tailored to your needs.




