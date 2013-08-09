# JasperReports Server on OpenShift

This git repository helps you get up and running a JasperReports Server installation
on OpenShift.  

By default the backend database is MySQL/Postgresql and the database name is the
same as your application name (using $_ENV['OPENSHIFT_APP_NAME']).  You can name
your application and database name to whatever you want.  

However, per default the name of the database will always match the application so you might have to update *.openshift/action_hooks/deploy*.

## Running on OpenShift

If you don't have an Openshift account you can create an account at http://openshift.redhat.com/ 

### Create a JBoss AS 7.1 or JBoss EAP 6.0 server

      **`JBoss AS 7.1`**

        rhc app create -a <my_app> -t jbossas-7  mysql-5.1 -g medium

        rhc app create -a <my_app> -t jbossas-7  postgresql-8.4  -g medium
	                    
      **`JBoss EAP 6.0`**

        rhc app create -a <my_app> -t jbosseap-6.0 mysql-5.1 -g medium
	
        rhc app create -a <my_app> -t jbosseap-6.0 postgresql-8.4  -g medium
    
* Important: For deploying JasperReports Server, the application max heap size should be set to 512m or above which is available only from medium and above gears, for more info check https://openshift.redhat.com/community/developers/pricing
		
## Application Info

          rhc app show -a <my_app>

## Add this upstream openshift-jasperreports repo

        cd <my_app>
        git remote add upstream -m master git://github.com/rudygodoy/openshift-jrs-quickstart
        git pull -s recursive -X theirs upstream master

  Then push the repo to your openshift cloud repo dir

        git push

   That's it, you can now checkout your application at:

        http://<my_app>-<my_namespace>.rhcloud.com
         
__Note__ : 
_Sometimes it takes longer time for the server to start and deploy, please watch the status of server on $OPENSHIFT_LOG_DIR/server.log.

## Application integration

If you are developing a product or managing an environment in which you want to integrate or embed BI, JasperReports Server provides several interfaces to allow you to do so quickly and easily. When integrating JasperReports Server into your environment or application, you will want to integrate two key areas: authentication and user interface.


### Capabilities

   * Single Sign On
   * HTTP APIs
   * User Interface Themes/skin
   * Web Services APIs

For more details, please visit: (http://www.jaspersoft.com/getting-started)


### Default Credentials

**Default Admin Username:** jasperadmin

**Default Admin Password:** jasperadmin


## JRS Database configuration

The JBoss AS 7.1 or JBoss EAP 6.0 cartridge will create two datasources by default one for MySQL and another for Postgesql.  The following are 
the JNDI names for the dataources
		MySQL      - java:jboss/datasources/MysqlDS
		PostgreSQL - java:jboss/datasources/PostgreSQLDS

By default this quick start will configure either MySQL/Postgresql database as database for the Portal depending on the DB cartridge that was added 
during the application creation.

This is configured in the $REPO_DIR/customization/jasperserver.war/WEB-INF/
	
	jdbc.default.jndi.name=java:jboss/datasources/MysqlDS
		
If you want to change it to Postgresql then you need to update the $REPO_DIR/customization/ROOT.war/WEB-INF/classes/portal-ext.properties

	jdbc.default.jndi.name=java:jboss/datasources/PostgreSQLDS

__Note__ : 

The datasources name could be changed by updting the file $APP_NAME/.openshift/config/standalone.xml and doing a git push
	
