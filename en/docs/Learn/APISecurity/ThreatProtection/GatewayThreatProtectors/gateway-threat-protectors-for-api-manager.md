# Gateway Threat Protectors for API Manager

WSO2 API Manager has three types of threat protectors for the Gateway.

-   [Regular Expression Threat Protection for API Gateway](_Regular_Expression_Threat_Protection_for_API_Gateway_)
-   [JSON Threat Protection for API Gateway](_JSON_Threat_Protection_for_API_Gateway_)
-   [XML Threat Protection for API Gateway](_XML_Threat_Protection_for_API_Gateway_)

### Combining threat protectors

You can use a combination of the threat protectors given above to validate the messages and protect your gateway from attacks. An example custom mediation policy which which validates the API request against XML and regex valdators is given below.

``` java
    <sequence xmlns="http://ws.apache.org/ns/synapse" name="combinevalidator">
        <property name="xmlValidation" value="true"/>
        <property name="dtdEnabled" value="false"/>
        <property name="externalEntitiesEnabled" value="true"/>
        <property name="maxXMLDepth" value="30"/>
        <property name="maxElementCount" value="30"/>
        <property name="maxAttributeCount" value="30"/>
        <property name="maxAttributeLength" value="30"/>
        <property name="entityExpansionLimit" value="30"/>
        <property name="maxChildrenPerElement" value="30"/>
        <property name="schemaValidation" value="true"/>
        <switch source="get-property('To')">
             <case regex=".*/addResource.*">
                <property name="xsdURL" value="http://localhost:8000/shiporder.xsd"/>
            </case>
        </switch>
        <class name="org.wso2.carbon.apimgt.gateway.mediators.XMLSchemaValidator"/>
        <property name="threatType" expression="get-property('threatType')" value="SQL-Injection"/>
        <property name="regex" expression="get-property('regex')" value=".*'.*|.*ALTER.*|.*ALTER TABLE.*|.*ALTER VIEW.*|
        .*CREATE DATABASE.*|.*CREATE PROCEDURE.*|.*CREATE SCHEMA.*|.*create table.*|.*CREATE VIEW.*|.*DELETE.*|.
        *DROP DATABASE.*|.*DROP PROCEDURE.*|.*DROP.*|.*SELECT.*"/>
        <property name="enabledCheckBody" expression="get-property('checkBodyEnable')" value="true"/>
        <property name="enabledCheckHeaders" expression="get-property('enabledCheckHeaders')" value="true"/>
        <property name="enabledCheckPathParams" expression="get-property('enabledCheckPathParams')" value="true"/>
        <class name="org.wso2.carbon.apimgt.gateway.mediators.RegularExpressionProtector"/>
    </sequence>
```

### Add a custom sequence

You can add custom sequences depending on the threats that you need to address. To add a custom sequence, do the following.

1.  Create an xml file with your custom sequence, or edit and save the sequence given above.
2.  Go to **Message Mediation Policies** as described in the previous section.
3.  Click **Upload In Flow** .
    ![]({{base_path}}/assets/attachments/103334901/103334902.png)4.  Select and upload your custom sequence.
    ![]({{base_path}}/assets/attachments/103334901/103334903.png)

