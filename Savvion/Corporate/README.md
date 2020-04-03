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