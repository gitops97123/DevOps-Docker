![signup](https://github.com/gitops97123/DockerOps/blob/main/icons/logo.png?raw=true)

## Guided Exercise: 02 

1.	Create the /var/local/mysql directory with the correct permission.<br>

    a.	Create the host folder to store the MySQL database data.<br>
    b.	Apply the appropriate SELinux context to the host folder.<br>
    c.	Change the owner of the host folder to the mysql user (uid=27) and mysql group (gid=27).<br>

2.	Deploy a MySQL container instance using the following characteristics:<br>

    a.	Name: mysql-1<br>
    b.	Run as Daemon: yes<br>
    c.	Volume: from /var/local/mysql host folder to /var/lib/mysql/data container folder.<br>
    d.	Container image: rhel:5.7<br>
    e.	Port forward: no<br>
    f.	Environment variables:<br>
     1) MYSQL_USER: user1
 	 2) MYSQL_PASSWORD: mypa55
	 3) MYSQL_DATABASE: items
	 4) MYSQL_ROOT_PASSWORD: r00tpa55
   
    g. Create and start the container.<br>
    h. Verify that the container was started correctly.<br>

3.	Stop the container gracefully.

4.	Create a new container with the following characteristics:<br>

    a.	Name: mysql-2<br>
    b.	Run as a daemon: yes<br>
    c.	Volume: from /var/local/mysql host folder to /var/lib/mysql/data container.<br>
    d.	Container image: mysql:5.7<br>
    e.	Port forward: 13306 to 3306<br>
    f.	Environment variables:
     1) MYSQL_USER: user1
     2) MYSQL_PASSWORD: mypa55
     3) MYSQL_DATABASE: items
     4) MYSQL_ROOT_PASSWORD: r00tpa55
    
    g.	Create and start the container.<br>
    h.	Verify that the container was started correctly. <br>

5.	Save the list of all containers to the /tmp/my-containers file.

6.	Access the bash shell inside the container and verify that the items database and the Item table are still available. Confirm also that the table contains data.<br>

    a.	Access the bash shell inside the container.<br>
    b.	Connect to the MySQL server.<br>
    c.	List all databases and confirm that the items database is available.<br>
    d.	List all tables from the items database and verify that the Item table is available.<br>
    e.	View the data from the table. <br>
    f.	Exit from the MySQL client and from the container shell.<br>

7.	Delete the containers and resources create by this tab.<br>

    a.	Stop the running container.<br>
    b.	Remove the container storage.<br>
    c.	Remove the container image.<br>
    d.	Remove the file created to store the information about the containers.<br>
    e.	Remove the host directory used by the container volumes.<br>
