# task_may11
The task list given on May 11 in theDevops Assembly Lines course

Task breakdown
* **task 1** - Create container image that has Jenkins installed  using dockerfile - check the Dockerfile in the repo. Base image is centos, on top of which java and jenkins have been installed, and port 8080 exposed
* **task 2** - When we launch this image, it should automatically starts Jenkins service in the container - achieved with the CMD option in the Dockerfile
* **task 3** - Create a job chain of job1, job2, job3 and  job4 using build pipeline plugin in Jenkins 
  * **Job 1** - Pull  the Github repo automatically when some developers push repo to Github - Achieved by configuring the gitweb-hook in this repo to trigger the jenkins job github-dl
  * **Job 2** - By looking at the code or program file, Jenkins should automatically start the respective language interpreter install image container to deploy code - The github-dl job triggers this downstream job, called start_site which finds out the files that have been changed in the upstream job, and based on the file extensions, re-creates the respective containers. 
    * Jenkins is in a container, so to create another container a named pipe has been used which connects jenkins and the base OS. Jenkins writes the docker commands to the pipe, which the base OS then executes
    * Only the html container recreation has been done till now, PHP is still pending
  * **Job 3 & 4** - Test your app if it  is working or not, and if not, then send a mail to the developer with the error messages -  Achieved with the downstream job called test_site, iswhich does a simple curl status check, and if the status is not 200, sends a mail via sendmail
  * **Job 5** - Monitor if container is up and running, and if it fails due to any reson then this job should automatically start the container again - achieved with the independent job monitor_site which runs every minute and checks if the container is running via docker ps (in turn via the named pipe), and recreates the container if it is down

