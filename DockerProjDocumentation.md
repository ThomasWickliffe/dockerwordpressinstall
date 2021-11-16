# Docker Project Documentation (WordPress)

### <u>Install the Docker and the Docker Compose
>`sudo apt update`</u>

>`sudo apt install ca-certificates curl gnupg lsb-release`

>`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | >sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg`

>`echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`

>`sudo apt update`

>`sudo apt install docker-ce docker-ce-cli containerd.io`

>`sudo usermod -aG docker admin`

>`sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`

>`sudo chmod +x /usr/local/bin/docker-compose`

>`docker-compose --version`

### <u> Create a directory to house WordPress </u>
Under the ***/srv***, place a new file named ***wordpress*** 

>` sudo mkdir -p /srv/wordpress`

### <u> Create the **docker-compose/yaml** file in the New Directory </u>

Enter the newly created file

>`cd /srv/wordpress`

Now use ***nano*** to edit the file and add in the configuration text

>`nano /srv/wordpress`

Here we are defining ***mysql*** and ***wordpress*** as the two services that are linked to each other. The image they use is the docker image that support the latest versions. Both services have the environment variables set to the users prefrences. For ***wordpress*** you need to set the port configuration, the fort used for this will be ***port 80*** and map to ***8080*** of the local computer

Add in the following text:

>version: '3' <br>
services: <br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;mysql: <br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;image: mysql:latest <br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;restart: always <br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;environment: <br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MYSQL_ROOT_PASSWORD: my_password<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MYSQL_DATABASE: wordpress<br>
     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MYSQL_USER: wordpress_user<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;volumes:<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- mysql_data:/var/lib/mysql<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;wordpress:<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;image: wordpress:latest<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;depends_on:<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  - mysql<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ports:<br>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- 8080:80<br>
   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;restart: always<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;environment:<br>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;WORDPRESS_DB_HOST: mysql:3306<br>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;WORDPRESS_DB_USER: wordpress_user<br>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;WORDPRESS_DB_PASSWORD: wordpress_password<br>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;volumes:<br>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- ./wp-content:/var/www/html/wp-content<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;volumes:<br>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; .mysql_data:<br>


### <u> Run WordPress </u>

Using Docker Compose while in the ***/srv/wordpress*** directory allows you to run WordPress in a local environment. 

>`sudo docker-compose up -d`

Allow time to download and install.
Using the web browser enter <br> 
>`http://localhost.8080`

The WordPress installation site will pop up and guide you throught the rest of the setup by asking for a language selection and login credentials.

WordPress should finally be up and running.