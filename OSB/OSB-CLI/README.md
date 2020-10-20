# OSB-CLI

### Local do binário do OSB-CLI
    C:\Tools\osb-cli.exe

### Create AS Proxy
    
Necessário estar dentro da pasta do projeto ApplicationServices, exemplo:

    C:\GIT-Rep\osb-wfm\ApplicationServices

Comando exemplo:

    C:\GIT-Rep\osb-wfm\ApplicationServices>c:\Tools\osb-cli.exe create proxy-as --application=ProcessInstance --proxy-name ProcessInstance70 --wsdl-path C:\Teste\AS\ProcessInstanceAPI.wsdl --osb-version 11g


### Create ES Proxy
    
Necessário estar dentro da pasta do projeto ES, exemplo:

    C:\GIT-Rep\osb-wfm\ES-Customer

Comandos exemplo:

    C:\GIT-Rep\osb-wfm\ES-Customer>c:\Tools\osb-cli.exe create proxy-es --tam-level ServiceMgmt/ServiceOrderManagement --proxy-name GetAvailableTasks --application-service ProcessInstance/ProcessInstance70/v1 --proxy-as-operation getAvailableTasks --description "Proxy ES de exemplo criado na apresentação de OSB-CLI" --osb-version 11G --osb-env WFM
    
    C:\GIT-Rep\osb-wfm\ES-Resource>c:\Tools\osb-cli.exe create proxy-es --tam-level /ResourceMgmt/NetworkNumberInventoryManagement --proxy-name ConfirmDisconnectNumber --application-service NumberInventory/NumberInventoryManagementWSV2/v1 --proxy-as-operation disconnectNumber --description "Proxy ES para confirmar a liberação de numberação no NumberInventory" --osb-version 11G --osb-env WFM