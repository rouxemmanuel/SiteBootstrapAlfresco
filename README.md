# SiteBootstrapAlfresco

## Description
This plugin allow to import a site creation on Alfresco startup but not only on alfresco installation or upgrade (like the "Sample: Web Site Design Project" site).
The creation used the patch process in order to not execute it several times.

This plugin allows to create one or more site(s) and for each site:
-  Create and import user(s) / group(s)
-  Set user(s) on group(s)
-  Import site content

The plugin includes a sample in the folder "src\main\config\alfresco\module\siteBootstrap\sample"


## Installation

- Get source from Git
- Run the following command: mvn clean compile package
- Copy the generated file "siteBootstrapAlfresco.amp" in the "amps" folder of your Alfresco installation
- Run the following command : ./apply_amps.sh (or apply_amps.bat)
- Create a file (for example in "tomcat/shared/classes/alfresco/extension/site-bootstrap-context.xml) with the following content:
```
<?xml version='1.0' encoding='UTF-8'?>
<!DOCTYPE beans PUBLIC '-//SPRING//DTD BEAN//EN' 'http://www.springframework.org/dtd/spring-beans.dtd'>
<beans>
  <!-- Site creation bootstrap -->
	<bean id="patch.siteLoadPatch.mySite" class="org.alfresco.plugin.site.bootstrap.SiteLoadBootstrap" parent="basePatch">
        <property name="id"><value>patch.siteLoadPatch.mySite</value></property>
        <property name="description"><value>patch.siteLoadPatch.description</value></property>
        <property name="disabled"><value>false</value></property>
        <property name="dependsOn" >
            <list>
                <ref bean="patch.updateDmPermissions" />
            </list>
        </property>
        <property name="spacesBootstrap" ref="siteLoadBootstrap-Spaces" />
        <property name="usersBootstrap" ref="siteLoadBootstrap-Users" />
        <property name="siteService" ref="siteService" />
        <property name="authorityService" ref="authorityService" />
        <property name="behaviourFilter" ref="policyBehaviourFilter" />
        <property name="siteName">
            <value>mySite</value>
        </property>
        <property name="bootstrapViews">
            <map>
            	<entry key="users">
                    <props>
                        <prop key="location">alfresco/extension/Users_mySite.acp</prop>
                    </props>
              </entry>
              <entry key="people">
                    <props>
                        <prop key="location">alfresco/extension/People_mySite.acp</prop>
                    </props>
              </entry>
              <entry key="groups">
                    <props>
                        <prop key="location">alfresco/extension/Groups_mySite.txt</prop>
                    </props>
              </entry>
              <entry key="contents">
                    <props>
                        <prop key="location">alfresco/extension/Content_mySite.acp</prop>
                    </props>
                </entry>
            </map>
        </property>
    </bean>
</beans>
```
- Restart Alfresco
- Your site will be created
