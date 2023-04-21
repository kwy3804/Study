## DB Lock 관련
**Lock 확인**<BR>						
EXEC SP_WHO2;<BR>				
BlkBy 컬럼에 값이 있으면 Lock이 걸린 프로세스						
						
EXEC SP_LOCK;<BR>	
Mode가 'X'인 데이터가 Lock이 걸린 프로세스						
						
SELECT * FROM SYS.sysprocesses WHERE BLOCKED > 0;<BR>					
blocked 컬럼에 값이 있으면 Lock이 걸린 프로세스<BR>						
						
**Lock 걸린 쿼리 확인**<BR>					
dbcc inputbuffer ( [spid] );						
						
**프로세스 KILL**<BR>						
EXEC KILL [spid]						

## 실행 계획 ##
SET SHOWPLAN_ALL ON;<BR>
GO<BR>
EXEC [스키마].[프로시저명] [파라미터1], [파라미터2], ...;<BR>
GO<BR>
SET SHOWPLAN_ALL OFF;<BR>
GO

## 쿼리 바로가기 ##
도구 → 옵션 → 환경 → 키보드 → 쿼리 바로 가기<BR>		
sp_help → 테이블 정보<BR>
sp_helptext → 프로시저 정보				
				
## 프로시저 내용 검색 ##
SELECT OBJECT_NAME(object_id), OBJECT_DEFINITION(object_id)<BR>				
FROM sys.procedures<BR>
WHERE OBJECT_DEFINITION(object_id) LIKE '%찾을내용%'				

## 세션 확인 ##
SELECT A.spid<BR>		
, A.login_time<BR>		
, A.loginame<BR>		
, A.last_batch<BR>		
, A.status<BR>		
, A.program_name<BR>		
, A.cmd<BR>		
, B.client_net_address<BR>		
FROM sys.sysprocesses A<BR> 		
JOIN sys.dm_exec_connections B<BR>		
ON A.spid = B.session_id<BR>		
ORDER BY spid		
