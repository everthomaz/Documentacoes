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


### Para configurar um repositório privado no Maven:

Abrir o arquivo pom.xml e adicionar antes da tag "dependencies":    

    <pluginRepositories>
    	<pluginRepository>
    		<id>al</id>
    		<url>http://99.99.99.99:8080/repository/maven-public/</url>
    		<name>private repository</name>
    	</pluginRepository>
    </pluginRepositories>
    <repositories>
    	<repository>
    		<id>al</id>
    		<url>http://99.99.99.99:8080/repository/maven-public/</url>
    		<name>private repository</name>
    	</repository>
    </repositories>