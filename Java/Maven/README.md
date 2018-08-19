# Maven

### Para configurar o proxy no Maven:

Configurar o arquivo: <home_user>/.m2/settings.xml
Se não existir o arquivo settings.xml, criar o arquivo.

    <settings>
    	<proxies>
    		<proxy>
    			<id>Nome de Indentificação</id>
    			<active>true</active>
    			<protocol>http</protocol>
    			<host>proxy.example.com</host>
    			<port>8080</port>
    			<username>username</username>
    			<password>password</password>
    			<nonProxyHosts>*10.*|*alsb*</nonProxyHosts>
    		</proxy>
    	</proxies>
    </settings>

