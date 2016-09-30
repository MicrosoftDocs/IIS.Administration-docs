---
uid: security/integrated/remove-integrated
---

# Removing IIS Integrated Auth

As specified in the [windows authentication](windows.md) and [authorization](authorization.md) sections, the Microsoft IIS Administration API is secured with windows authentication and RBAC (role-based access control). Some use cases may warrant removing these requirements and using only access tokens for security. To do this, the [web.config](web.config.md) file must be modified to disable the [windows authentication](windows.md) and [authorization](authorization.md) requirements.

**Note:**
The [web.config](web.config.md) file contains settings for the /security route marked with the `<location path="security">` tag. These settings should not be modified because they secure the generation of access tokens.

The web.config file comes with the following settings

    <security>
      <authentication>
        <!-- Anonymous Authentication 
        <anonymousAuthentication enabled="true" />
         -->
        <!-- Windows Authentication -->
        <windowsAuthentication enabled="true" />
      </authentication>
      <authorization>
        <clear />
        <!-- Anonymous Authentication 
        <add accessType="Allow" users="*"/>
        -->
        <!-- Windows Authentication -->
        <add accessType="Allow" roles="Administrators,IIS Administrators" />
        <!-- Read Only Deployment
        <remove users="*"/>
        <add accessType="Allow" users="*" verbs="GET,HEAD,OPTIONS" />
         -->
      </authorization>
    </security>

To remove the integrated auth requirements, modify the settings to 

    <security>
      <authentication>
        <!-- Anonymous Authentication -->
        <anonymousAuthentication enabled="true" />        
        <!-- Windows authentication
        <windowsAuthentication enabled="true" />
        -->
      </authentication>
      <authorization>
        <clear />
        <!-- Anonymous Authentication -->
        <add accessType="Allow" users="*"/>
        <!-- Windows Authentication
        <add accessType="Allow" roles="Administrators,IIS Administrators" />
        -->
        <!-- Read Only Deployment
        <remove users="*"/>
        <add accessType="Allow" users="*" verbs="GET,HEAD,OPTIONS" />
         -->
      </authorization>
    </security>
