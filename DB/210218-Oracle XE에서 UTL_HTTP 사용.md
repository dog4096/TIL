# Oracle XE에서 UTL_HTTP 사용
DB변경 감지 Trigger -> Procedure -> UTL_HTTP에서 API 호출이라는 루틴을 만들기 위해  
먼저 UTL_HTTP를 사용 할 수 있는지 알아보려고 했다.

## 1. 권한 부여  
먼저 UTL_HTTP를 Procedure에서 사용해보려고 하면 
```
network access denied by access control list (ACL)
```
에러를 내뱉는다.  

어떻게든 이를 해결하기 위해 여러가지를 찾아보았는데 아직 Oracle자체가 익숙하지도 않고 하다보니  
정작 완벽히 이해는 못한 상태서 해결방법은 찾긴 했다.
먼저 해당 내용은 SYS 계정으로 진행하였다.
 > TIP: SYS계정이 접속이 안되는 문제가 있어 계정명을 sys as sysdba로 접속했다.

```oraclesqlplus
begin
     DBMS_NETWORK_ACL_ADMIN.create_acl (
        acl => 'network_services.xml', -- 다른데선 경로가 지정되어있고 그랬는데 이게 사용자 커스텀인지 정해진 값이 있는건지 모르겠다.
        description => 'TEST CONNECT', -- 설명
        principal => '권한을 부여할 계정',
        is_grant => TRUE,
        privilege => 'connect',
        start_date   => SYSTIMESTAMP
     );
     commit;
end;

begin
    DBMS_NETWORK_ACL_ADMIN.ADD_PRIVILEGE (
            acl => 'network_services.xml',
            principal => '권한을 부여할 계정',
            is_grant => TRUE,
            privilege => 'resolve'
        );
    commit;
end;

BEGIN
    dbms_network_acl_admin.assign_acl (
         acl => 'network_services.xml',
        host => '*' -- 특정 호스트로만 사용할 수 있게 지정가능
    );
    commit;
END;
```

해당 내용을 실행한 후 

```oraclesqlplus
SELECT * FROM DBA_NETWORK_ACLS
```
쿼리를 실행해보면 HOST와 ACL이 지정된걸 확인할 수 있다.

## 2. Procedure 작성

```oraclesqlplus
create procedure TEST_HTTP_CALL IS
    REQ UTL_HTTP.req;
    RES UTL_HTTP.RESP;
BEGIN
    UTL_HTTP.SET_TRANSFER_TIMEOUT(5);
    REQ := UTL_HTTP.BEGIN_REQUEST('호출할 URL', 'GET');
    UTL_HTTP.SET_HEADER(REQ, 'User-Agent', 'Mozilla/4.0');
    RES := UTL_HTTP.GET_RESPONSE(REQ);
    DBMS_OUTPUT.PUT_LINE('HTTP response status code: ' || RES.status_code);
END TEST_HTTP_CALL;
/
```
다시 권한을 부여한 계정으로 돌아와서 해당 Procedure를 만든 후 실행해보자.
> begin
>   TEST_HTTP_CALL;
> end;
> [2021-02-18 15:04:42] completed in 945 ms

실행이 잘 되었다.