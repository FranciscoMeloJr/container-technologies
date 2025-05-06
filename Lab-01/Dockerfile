# Example for EAP image build:
# Use EAP 8 Builder image to create a JBoss EAP 8 server with its default configuration
FROM registry.redhat.io/jboss-eap-8/eap8-openjdk17-builder-openshift-rhel8:latest AS builder
ENV GALLEON_PROVISION_FEATURE_PACKS=org.jboss.eap:wildfly-ee-galleon-pack,org.jboss.eap.cloud:eap-cloud-galleon-pack
ENV GALLEON_PROVISION_LAYERS=jaxrs-server,cloud-default-config,-jpa
ENV GALLEON_PROVISION_CHANNELS=org.jboss.eap.channels:eap-8.0
ENV GALLEON_MAVEN_ARGS="-gs /opt/jboss/container/wildfly/s2i/galleon/provisioning/global-setting.xml -Dmaven.repo.local=/tmp/artifacts/tmp/m2" 
ENV SCRIPT_DEBUG false
COPY ./global-setting.xml /opt/jboss/container/wildfly/s2i/galleon/provisioning/global-setting.xml
LABEL MAINTAINER="francisco at red hat dot com"
RUN /usr/local/s2i/assemble
RUN ls /tmp/artifacts/tmp/m2


## EAP 8 Runtime image
FROM registry.redhat.io/jboss-eap-8/eap8-openjdk17-runtime-openshift-rhel8:latest AS runtime
COPY --from=builder --chown=jboss:root $JBOSS_HOME $JBOSS_HOME

##############################################################################################
#
# Steps to add:                                                                                    
# (1) COPY or ADD the WAR/EAR to $JBOSS_HOME/standalone/deployments. 
#       For example:
#       COPY --chown=jboss:root test.war $JBOSS_HOME/standalone/deployments                                
# (2) (Optional) Modify the $JBOSS_HOME/standalone/configuration/standalone.xml (not standalone-openshift.xml)
# (3) (Optional) set ENV variable CONFIG_IS_FINAL to true if no modification is needed by start up scripts. 
#       For example:
#       ENV CONFIG_IS_FINAL=true 
# (4) (Optional) copy a modified standalone.xml in $JBOSS_HOME/standalone/configuration/
#       For example:
#       COPY --chown=jboss:root standalone.xml  $JBOSS_HOME/standalone/configuration
##############################################################################################

RUN chmod -R ug+rwX $JBOSS_HOME
