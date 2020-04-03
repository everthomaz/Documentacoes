# Savvion

### Grant Savvion Corporate
    -- SAVVION CORPORATE - Legado
    -- INSTANCIAS: PSOAR1
    -- AMBIENTE: PRODUÇÃO
    -- OWNER: SAVVIONCORP_OWNER
    
    -- SAVVION CORPORATE - Legado
    -- INSTANCIAS: QA3SOA
    -- AMBIENTE: ESTEIRAS/PRE-PROD
    -- OWNER: SAVVIONCORP_OWNER 

    Set trims on
    Set echo on
    Set feed on
    Set timi on
    Set time on

    Spool script_Grant_SavvionCorporate.log

    EXECUTE SAVVIONCORP_OWNER.GENERIC_GRANT_TO_USER('SAVVIONCORP_OWNER', 'PROCESS_INSTANCE_PROD');
    EXECUTE SAVVIONCORP_OWNER.GENERIC_GRANT_TO_USER('SAVVIONCORP_OWNER', 'BPM_PORTAL_PROD');

    Spool off
    Exit



### Queries úteis

##### Processamento de eventos

-- BIZEVENT: Tabela onde ficam todos os eventos.

    SELECT * FROM SAVVIONCORP_OWNER.BIZEVENT;
    SELECT MAX(EVENT_ID) FROM SAVVIONCORP_OWNER.BIZEVENT;

-- BIZSTOREEVENTCOUNTER: Tabela guarda o último evento processado.

    SELECT * FROM SAVVIONCORP_OWNER.BIZSTOREEVENTCOUNTER;

<br/>

##### Localiza a instância através do nome do componente e um dataslot.
--Componente: SERVICEORDERMNGMT_V10<br/>
--Dataslot: PURCHASEORDERNUMBER

    select PROCESS_INSTANCE_ID from SERVICEORDERMNGMT_V10 where PURCHASEORDERNUMBER = '1-11839935972';

<br/>

##### Adianta uma tarefa que está aguardando um tempo de execução (timer).

    update BIZLOGIC_TIMERACTION set duedate = 0 where process_instance_id = '335716';

<br/>

##### Query busca URL:
    
    SELECT 'http://svuxqa6soa64:8080/sbm/bpmportal/myhome/psv_table_view.jsp?PTID='||PT.PROCESS_TEMPLATE_ID||'&'||'PIID='||PI.PROCESS_INSTANCE_ID||'&' ||'piName='||RAWTOHEX(PT.PROCESS_TEMPLATE_NAME||'#'||PI.PROCESS_INSTANCE_ID)||'&'||'link=status'           FROM savvioncorp_owner.PROCESSINSTANCE PI           INNER JOIN savvioncorp_owner.PROCESSTEMPLATE PT ON PT.PROCESS_TEMPLATE_ID = PI.PROCESS_TEMPLATE_ID           WHERE PI.PROCESS_INSTANCE_ID = (select PROCESS_INSTANCE_ID from savvioncorp_owner.serviceordermngmt_v10 where purchaseordernumber = '1-11847386176');