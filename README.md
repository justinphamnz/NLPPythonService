# NLPPythonService
Python service written on Python with all virtual environment setup, ready to deploy to IIS or Azure Web App

# env folder need to be replaced direct to source code through FTP, it will reduce a lot of hassle on server.
-Install all required packages 
--Window environment required wheel installed cd C:/Project/Basket/env/Scripts pip install wheel
--In order to install scikit-learn, numpy+mkl wheel need to be installed and then install scipy otherwise, it's no complier supported for scipy on Window
--Install scikit-learn. 
--All installation should be done on local virtual environment and ship it to env on server. 

# Note for web.config
<appSettings>
    <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="FlaskWebProject.app" /> // value will change accorddingly to name of main app
    <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS" value="%ROOTDIR%\env\Scripts\activate_this.py" />
    <add key="WSGI_HANDLER" value="ptvs_virtualenv_proxy.get_virtualenv_handler()" />
    <add key="" value="%ROOTDIR%" />
  </appSettings>
  <system.web>
    <compilation debug="true" targetFramework="4.0" />
  </system.web>
  <system.webServer>
    <modules runAllManagedModulesForAllRequests="true" />
    <handlers>
      <add name="Python FastCGI" path="handler.fcgi" verb="*" modules="FastCgiModule" scriptProcessor="%INTERPRETERPATH%|%WFASTCGIPATH%" resourceType="Unspecified" requireAccess="Script" />
    </handlers>
    <rewrite>
      <rules>
        <rule name="Static Files" stopProcessing="true">
          <match url="^/static/.*" ignoreCase="true" />
          <action type="Rewrite" url="^/FlaskWebProject/static/.*" appendQueryString="true" />
        </rule>
        <rule name="Configure Python" stopProcessing="true">
          <match url="(.*)" ignoreCase="false" />
          <conditions>
          </conditions>
          <action type="Rewrite" url="handler.fcgi/{R:1}" appendQueryString="true" />
        </rule>
      </rules>
    </rewrite>
  </system.webServer>
  
  

