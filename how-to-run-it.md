# How to just run it...

For this project:  
[Get Started Building Microservices with ASP.NET Core and Docker in Visual Studio Code](https://fullstackmark.com/post/12/get-started-building-microservices-with-asp.net-core-and-docker-in-visual-studio-code)

Code:  
https://github.com/mmacneil/ASPNETCoreDockerMicroservices

Since debugging did not work, I wanted to just get this project up and running. 

## Git Bash

- Execute `docker-compose up --force-recreate -d` (no need to remove/tear down containers, just rerun this command and everything gets recreated (including the databases)!)
  - `docker-compose ps` to check if all containers start
  - if there are port conflicts, change the ports in [docker-compose.yml](docker-compose.yml)
- Connect to `mssql-linux` container with SSMS/LinqPad
  - localhost,5433
  - sa / Pass@word
  - ensure the databases `dotnetgigs.applicants` and `dotnetgigs.jobs` were created
  - if not, execute the following commands manually (then verify again):
    - $ `docker exec -it mssql-linux sh`
    - \# `chmod +x ./SqlCmdStartup.sh`
    - \# `/bin/bash`
    - root@75d4559287eb:/# `./entrypoint.sh`
    - root@75d4559287eb:/# `exit`
    - \# `exit`

The databases should now be up and running. Time to start the projects...

From the **solution root**: 
- create a **new bash session for each of the following commands**:
  - `docker exec -i -t applicants.api sh -c "cd app ; dotnet applicants.api.dll"`
  - `docker exec -i -t jobs.api sh -c "cd app ; dotnet jobs.api.dll"`
  - `docker exec -i -t identity.api sh -c "cd app ; dotnet identity.api.dll"`
  - `docker exec -it web sh -c "dotnet web.dll"`

Now http://localhost:8080 should work. Otherwise check the output in the different bash windows to get more information.

# Clean up

Stop everything:  
- CTRL+C all command windows
- `docker-compose down` (takes a while to complete).

Or:  
- `docker-compose up --force-recreate -d` to just recreate all the images, without having to remove them first (no reuse!).

To remove individual images:
- `docker rmi mssql-linux`
- `docker rmi web`
- `docker rmi applicants.api`
- `docker rmi jobs.api`
- `docker rmi identity.api`

# 2nd try, using PowerShell

`docker-compose up -d` seems to be successful this time, databases got created!

`docker-compose up --force-recreate -d`

# ToDo: add ENTRYPOINT to Dockerfiles to start without commands


# Links

- http://masstransit-project.com/
- https://mherman.org/blog/dockerizing-an-angular-app/
- https://malcoded.com/posts/angular-docker
- https://medium.com/@DenysVuika/your-angular-apps-as-docker-containers-471f570a7f2
- https://letsencrypt.org/getting-started/