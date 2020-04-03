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
    


