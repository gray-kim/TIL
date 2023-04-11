# Oracle Lock걸린 계정 확인 및 Kill

```sql
-- lock걸린 객체 확인
SELECT OBJECT_ID
     , SESSION_ID       -- SID
     , ORACLE_USERNAME
     , OS_USER_NAME
  FROM V$LOCKED_OBJECT
;

-- 해당 sid 와 serial 번호로 락걸린 object name 을 확인
SELECT A.SID
     , A.SERIAL#
     , object_name
     , A.SID || ', ' || A.SERIAL# AS KILL_TASK
  FROM V$SESSION A
 INNER JOIN V$LOCK B
    ON A.SID = B.SID
 INNER JOIN DBA_OBJECTS C
    ON B.ID1 = C.OBJECT_ID
 WHERE B.TYPE  = 'TM'
 ;


CALL rdsadmin.rdsadmin_util.kill(51, 26804);
CALL rdsadmin.rdsadmin_util.kill(58, 7076);
```

```sql
-- 기타 항목 조회
SELECT s.sid,
       s.serial#,
       s.process,
       s.osuser,
       s.program
FROM   v$session s;

SELECT a.sid, a.serial#, a.status, a.process, a.username, a.osuser, b.sql_text, c.program
FROM v$session a, v$sqlarea b, v$process c
WHERE a.sql_hash_value=b.hash_value
  AND a.sql_address=b.address
  AND a.paddr=c.addr
  AND a.status='ACTIVE'
;
```