## DB Lock 관련
**Lock 확인**
```sql
EXEC SP_WHO2;			
```
BlkBy 컬럼에 값이 있으면 Lock이 걸린 프로세스

```sql
EXEC SP_LOCK;	
```
Mode가 'X'인 데이터가 Lock이 걸린 프로세스				
						
```sql
SELECT * FROM SYS.sysprocesses WHERE BLOCKED > 0;	
```
blocked 컬럼에 값이 있으면 Lock이 걸린 프로세스				
						
**Lock 걸린 쿼리 확인**
```sql
dbcc inputbuffer ( [spid] );
```
						
**프로세스 KILL**
```sql
EXEC KILL [spid];
```

## 실행 계획 ##
```sql
SET SHOWPLAN_ALL ON;
GO
EXEC [스키마].[프로시저명] [파라미터1], [파라미터2], ...;
GO
SET SHOWPLAN_ALL OFF;
GO
```

## 쿼리 바로가기 ##
도구 → 옵션 → 환경 → 키보드 → 쿼리 바로 가기  
sp_help → 테이블 정보  
sp_helptext → 프로시저 정보				
				
## 프로시저 내용 검색 ##
```sql
SELECT OBJECT_NAME(object_id), OBJECT_DEFINITION(object_id)				
FROM sys.procedures
WHERE OBJECT_DEFINITION(object_id) LIKE '%찾을내용%';
```

## 세션 확인 ##
```sql
SELECT A.spid	
, A.login_time	
, A.loginame	
, A.last_batch	
, A.status
, A.program_name	
, A.cmd
, B.client_net_address		
FROM sys.sysprocesses A	
JOIN sys.dm_exec_connections B		
ON A.spid = B.session_id
ORDER BY spid
```

## 실행중인 쿼리 확인 ##
```sql
SELECT				
   sqltext.TEXT,				
   req.session_id,				
   req.status,				
   req.command,				
   req.cpu_time,				
   req.total_elapsed_time				
FROM sys.dm_exec_requests req					
CROSS APPLY sys.dm_exec_sql_text(sql_handle) AS sqltext 					
```
