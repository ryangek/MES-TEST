# REQUIREMENT 4 : WIP Transactions

## 1) Find Operation

```sql
select distinct(a.operationname) from (select op.OPERATIONNAME,wb.WORKFLOWNAME,
case substr(workflowname,1,2)
 when 'WT' then 'WT'
 when 'TW' then 'WT'
 when 'AS' then 'AS'
 when 'A_' then 'AS'
 when 'FT' then 'FT'
 when 'KP' then 'PK'
 when 'KT' then 'PK'
 when 'K_' then 'PK'
 when 'LA' then 'AS'
 when 'PA' then 'PA'
 when 'PH' then 'PA'
 when 'PP' then 'PK'
 when 'T_' then 'FT'
end as Phase
from workflowbase wb, workflow wf, workflowstep ws, path pt, workflowstep wst, spec sp,specbase sb, operation op
where wb.workflowbaseid = wf.workflowbaseid
and wf.WORKFLOWID = ws.WORKFLOWID
and ws.DEFAULTPATHID = pt.PATHID (+)
and pt.TOSTEPID = wst.WORKFLOWSTEPID (+)
AND DECODE(ws.SPECID,'0000000000000000',ws.SPECBASEID,ws.SPECID) = DECODE(ws.SPECID,'0000000000000000',sp.SPECBASEID(+),sp.SPECID(+))
AND sp.SPECBASEID = sb.SPECBASEID(+)
and sp.OPERATIONID = op.OPERATIONID (+)
--and wb.workflowname not in ('A_TOOK','Auto Material','Auto Material Withdrawal','DIE_STORE',
--'DMaterialInvAS','DummyML','ICAM_STORE','ICFT_STORE','PEP_STORE','Security Product Scrap',
--'TW_STORE','WT001TG100','zTT-VAG','WAFER_STORE','A_SF1B26_TOOK')
and substr(wb.workflowname,1,3) <> 'OB-') a
where phase = 'FT';
```

## 2) WIP TRANSACTIONS

- PHASE : `WAFER TEST_WT`
- PHASE : `PRE ASSEMBLY_PA`
- PHASE : `ASSEMBLY_AS`
- PHASE : `FINAL TEST_FT`

### 2.1) START

```
OPEN BROWSER(GET SERVERNAME : hgbnklak1vwtu1
    LOGIN (GET USERNAME/PASSWORD)
        OPEN MES_CMSS WIP TRACKING
            SCAN LOT(GET LOTNAME)
                IF  CURRENT_SPEC IN WT
                    {
                        IF CURRENT_SPEC !=  TW_STORE

                        FOR /*LOOP TO SUBMIT MOVE IN/TRACK IN/TRACK OUT/MOVE OUT
                                    (SUBMIT = 0; SUBMIT+=1;SUBMIT <=9999)
                                {CHECK_EQUIPMENT
                                    IF EQUIPMENT = YES AND WIP_DATA = NO
                                        ADD MATERIAL;/*GET MATERIAL_ID
                                        CLICK_SUBMIT;
                                    ELSE IF EQUIPMENT = NO AND WIP_DATA = YES
                                        ADD WIP_DATA;
                                        CLICK_SUBMIT;
                                    ELSE IF EQUIPMENT = NO AND WIP_DATA = NO
                                        CLICK_SUBMIT;

                                    SUBMIT = SUBMIT+1;
                                }
                                END

                        ELSE IF CURREN_SPEC = TW_STORE
                                END
                    }

                ELSE IF CURRENT_SPEC IN PA

                    {IF CURRENT_SPEC != DIE_STORE

                        FOR /*LOOP TO SUBMIT MOVE IN/TRACK IN/TRACK OUT/MOVE OUT
                                    (SUBMIT = 0; SUBMIT+=1;SUBMIT <=9999)
                                {CHECK_EQUIPMENT
                                    IF EQUIPMENT = YES AND WIP_DATA = NO
                                        ADD MATERIAL;/*GET MATERIAL_ID
                                        CLICK_SUBMIT;
                                    ELSE IF EQUIPMENT = NO AND WIP_DATA = YES
                                        ADD WIP_DATA;
                                        CLICK_SUBMIT;
                                    ELSE IF EQUIPMENT = NO AND WIP_DATA = NO
                                        CLICK_SUBMIT;

                                    SUBMIT = SUBMIT+1;
                                }
                                END

                    ELSE IF CURRENT_SPEC = DIE_STORE
                                END
                    }

                ELSE IF CURRENT_SPEC = AS

                        {IF CURRENT_SPEC != PEP_STORE

                        FOR /*LOOP TO SUBMIT MOVE IN/TRACK IN/TRACK OUT/MOVE OUT
                                    (SUBMIT = 0; SUBMIT+=1;SUBMIT <=9999)
                                {CHECK_EQUIPMENT
                                    IF EQUIPMENT = YES AND SZWIP_DATA = NO
                                        ADD MATERIAL;/*GET MATERIAL_ID
                                        CLICK_SUBMIT;
                                    ELSE IF EQUIPMENT = NO AND WIP_DATA = YES
                                        ADD WIP_DATA;
                                        CLICK_SUBMIT;
                                    ELSE IF EQUIPMENT = NO AND WIP_DATA = NO
                                        CLICK_SUBMIT;

                                    SUBMIT = SUBMIT+1;
                                }
                                END

                    ELSE IF CURRENT_SPEC = PEP_STORE
                                END
                    }

                ELSE IF CURRENT_SPEC = FT
                {IF CURRENT_SPEC != ICFT_STORE

                        FOR /*LOOP TO SUBMIT MOVE IN/TRACK IN/TRACK OUT/MOVE OUT
                                    (SUBMIT = 0; SUBMIT+=1;SUBMIT <=9999)
                                {CHECK_EQUIPMENT
                                    IF EQUIPMENT = YES AND SZWIP_DATA = NO
                                        ADD MATERIAL;/*GET MATERIAL_ID
                                        CLICK_SUBMIT;
                                    ELSE IF EQUIPMENT = NO AND WIP_DATA = YES
                                        ADD WIP_DATA;
                                        CLICK_SUBMIT;
                                    ELSE IF EQUIPMENT = NO AND WIP_DATA = NO
                                        CLICK_SUBMIT;

                                    SUBMIT = SUBMIT+1;
                                }
                                END

                    ELSE IF CURRENT_SPEC = ICFT_STORE
                                END
                    }
END
```

## Hello World
