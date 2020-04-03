# Savvion

### Grant Savvion Billing Corporate
    
    -- SAVVION BILLING CORPORATE - Legado
    -- INSTANCIAS: PBLCR
    -- AMBIENTE: PRODUÇÃO
    -- OWNER: SAVVION_BILCORP_OWNER
    
    -- SAVVION BILLING CORPORATE - Legado
    -- INSTANCIAS: QA3SOA
    -- AMBIENTE: ESTEIRAS/PRE-PROD
    -- OWNER: SAVVION_BILCORP_OWNER

    Set trims on
    Set echo on
    Set feed on
    Set timi on
    Set time on

    Spool script_Grant_SavvionBillingCorporate.log

    EXECUTE SAVVION_BILCORP_OWNER.GENERIC_GRANT_TO_USER('SAVVION_BILCORP_OWNER', 'PROCESS_INSTANCE_PROD'); 
    EXECUTE SAVVION_BILCORP_OWNER.GENERIC_GRANT_TO_USER('SAVVION_BILCORP_OWNER', 'BPM_PORTAL_PROD');

    Spool off
    Exit



