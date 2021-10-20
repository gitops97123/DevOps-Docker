![signup](https://github.com/gitops97123/DockerOps/blob/main/icons/logo.PNG?raw=true)

### NOTE: After used Container, please remove its container ids. 

### Guided Exercise: 03
 						
1.	Create a MySQL container image with 5.7 tag,
o	Start a container from mysql image
    - 	MYSQL_USER=devops
    - 	MYSQL_PASSWORD=mypa55
    - 	MYSQL_DATABASE=titanic
    - 	MYSQL_ROOT_PASSWORD=r00tpa55
    - 	 Verify that the container started without errors.
    - 	Access the container 
    - 	Add data to the database & create a new table in the titanic database. 

2.	Start a container named httpd-basic in the background, and forward port 8080 to port 80 in the container. Use the httpd container image with 2.4 tag. 
- 	Test the httpd-basic container.
    -	From base machine, attempt to access http://localhost:8080 using any web browser. An “it works” message is displayed, which is the index.html page from apache HTTP server container running on base machine.
-	Customize the httpd-basic container.
    -	Start a bash session inside the container and customize the existing content of index.html page.
    -	From the bash session, verify the index.html file under /usr/local/apache2/htdocs directory using the ls -la command.
    -	Change the index.html page to contain the text Hello world, replacing all of the existing content
    -	Attempt to access http://localhost:8080 again, and verify that the web page has been updated.

###	Clean up.




