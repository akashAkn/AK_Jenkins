**BUILD and DEPLOYMENT:**
**BUILD PROCEDURE STEPS:**
**1.	ECR/Docker login**
source ~/.bash_profile
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 437715327374.dkr.ecr.us-east-1.amazonaws.com
Pulling from hp2b/ts-utils
docker pull 437715327374.dkr.ecr.us-east-1.amazonaws.com/hp2b/ts-utils:9.1.8.1
Tagging image to hp2b/ts-utils:9.1.8.1
docker tag 437715327374.dkr.ecr.us-east-1.amazonaws.com/hp2b/ts-utils:9.1.8.1 hp2b/ts-utils:9.1.8.1
**2.	run WC Build in Docker**
Running the HCL Commerce build inside the container.
APP_TYPE     = ts
BRANCH_NAME  = release_fy23_pi2

gulpfile.js, wcbd-build.xml  – gulp file will be executed

wcbd-deploy-server-local-ts-release_fy23_pi2.zip  -- package will be created

**3.	Unzip WCB package**
Unzipping WCB package /opt/JenkinsHome/workspace/Build_-_TS-App_release_fy23_pi2/11/package/wcbd-deploy-server-local-ts-release_fy23_pi2.zip

**4.	Commerce Docker Image**
Building docker image 437715327374.dkr.ecr.us-east-1.amazonaws.com/hp2b/ts-app:release_fy23_pi2

**5.	Remove WCB package**
Removing /opt/JenkinsHome/workspace/Build_-_TS-App_release_fy23_pi2/11/package/wcbd-deploy-server-local-ts-release_fy23_pi2.zip

**WCB DATALOAD:**
All sql file in common / Evironment folders will be executed in Data base:
https://github.azc.ext.hp.com/HP2B/HCLCOM9/blob/release_fy23_pi1/workspace/Dataload/sql/FY23_PI1/common/
https://github.azc.ext.hp.com/HP2B/HCLCOM9/blob/release_fy23_pi1/workspace/Dataload/sql/FY23_PI1/Environement

**WCB Ts- Utils:**
https://github.azc.ext.hp.com/HP2B/HCLCOM9/blob/release_fy23_pi1/workspace/Dataload/sql-archive/sql/dev2/jukebox/ 


**TAG AND DEPLOYMENT:**
Runs as wcsadmin and perform - source ~/.bash_profile
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 437715327374.dkr.ecr.us-east-1.amazonaws.com

docker pull 437715327374.dkr.ecr.us-east-1.amazonaws.com/hp2b/ts-app:v9-migration 
docker pull 437715327374.dkr.ecr.us-east-1.amazonaws.com/hp2b/ts-web:v9-migration 
docker pull 437715327374.dkr.ecr.us-east-1.amazonaws.com/hp2b/search-app:v9-migration 

docker tag 437715327374.dkr.ecr.us-east-1.amazonaws.com/hp2b/ts-app:v9-migration  437715327374.dkr.ecr.us-east-1.amazonaws.com/hp2b/ts-app:itg2live
docker tag 437715327374.dkr.ecr.us-east-1.amazonaws.com/hp2b/ts-web:v9-migration  437715327374.dkr.ecr.us-east-1.amazonaws.com/hp2b/ts-web:itg2live
docker tag 437715327374.dkr.ecr.us-east-1.amazonaws.com/hp2b/search-app:v9-migration  437715327374.dkr.ecr.us-east-1.amazonaws.com/hp2b/search-app:itg2live

docker push 437715327374.dkr.ecr.us-east-1.amazonaws.com/hp2b/ts-app:itg2live 
docker push 437715327374.dkr.ecr.us-east-1.amazonaws.com/hp2b/ts-web:itg2live
docker push 437715327374.dkr.ecr.us-east-1.amazonaws.com/hp2b/search-app:itg2live

**then we will proceed with helm upgrade:**

helm upgrade hp2b-itg2-share ./ -f itg2_live_deployment_share.yaml -n commerce --set common.environmentType=share { subsequent deployments }

If it is a new environment we need to perform helm istall:
helm install hp2b-itg2-share ./ -f itg2_live_deployment_share.yaml -n commerce --set common.environmentType=share { first time deployment }

PATH: /opt/DEPLOY/HelmCharts
yaml file -- pro-stg_auth_deployment.yaml (eg) – Environment details will be present in this yaml file
in templates we can find the pod details like livesness, readiness probe stats for configure pods.

**For deployment :**
deploymnent pods will alone be executed.
Statefullset will not be affected.

