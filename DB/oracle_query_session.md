# 실행중인 쿼리 조회 및 kill

```sql
SELECT a.osuser
               ,a.SID
               ,a.serial#
               ,a.status
               ,b.sql_text
  FROM v$session a
              ,v$sqlarea b
WHERE a.sql_address = b.address;

SELECT
  a.sid,       -- SID
  a.serial#,   -- 시리얼번호
  a.status,    -- 상태정보
  a.process,   -- 프로세스정보
  a.username,  -- 유저
  a.osuser,    -- 접속자의 OS 사용자 정보
  b.sql_text,  -- sql
  c.program    -- 접속 프로그램
FROM
  v$session a,
  v$sqlarea b,
  v$process c
WHERE
  a.sql_hash_value=b.hash_value
  AND a.sql_address=b.address
  AND a.paddr=c.addr
  AND a.status='ACTIVE';

--유저 세션 KILL
ALTER SYSTEM KILL SESSION 'SID,시리얼번호';
```