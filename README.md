🙆‍♂️오라클 팀 프로젝트🙆‍♂️

![image](https://user-images.githubusercontent.com/85034286/153702662-7a8446de-9e1c-4348-918a-e49cb78dcecd.png)

>  해당 프로젝트는 6명의 팀원이 약 5일간 진행한 오라클기반의 학원 관리 프로젝트 입니다. <br />

<br />

# 📌 Table Of Contents
* [📖 Introduction](#-introduction)
* [🙋 My Role](#-my-role)
* [🔎 Detail](#-detail)
* [💡 Review](#-review)

<br />
<br />
<br />



# 📖 Introduction
### 1. 프로젝트 개요
* 교육 센터를 운영함에 있어 필요한 제반 기능들을 하나의 프로그램으로 관리할 수 있다.

* 교육생들의 교육 환경 조성을 위하여 교육 제반 행정 사항들을 컨트롤할 수 있다.
* 교육생들간 커뮤니티 개설을 통해 인적 네트워크를 활성화 할 수 있다.

<br />

### 2. 개발 환경
<img src="https://img.shields.io/badge/oracle-F80000?style=for-the-badge&logo=oracle&logoColor=white">

* 본 프로젝트는 오라클과 오라클 기능중 PL/SQL을 주로 사용하여 진행했습니다.
* ORACLE Developer를 이용하였고 , ERD 작성으로는 exERD를 사용하였습니다.
* oracle에서 commit 하면 서버에 정보가 저장되기 때문에 따로 형상관리툴을 사용하지 않았습니다.
<br />

### 3. 프로젝트 내용
![image](https://user-images.githubusercontent.com/85034286/153702031-0989fcd3-ac85-41ad-af2c-19e5aaa1fbcf.png)

#### 3-1. 관리자
* 기초 정보 관리
* 계정 관리
* 출결 관리
* 교사 및 교육생 관리
* 개설 과정 및 과목 관리
* 면접 관리
* 교육지원금 관리

#### 3-2. 교사
* 강의 스케줄 조회
* 본인 정보 조회
* 학생 정보 조회 
* 담당 과정 관리
* 성적 관련 업무 
* 교육생 관리

#### 3-3. 교육생
* 수강 정보 조회
* 교육 지원금 조회
* 프로젝트 공고 모집
* 교사 평가
* 사후 처리 입력
* Q&A

<br />
<br />
<br />



# 🙋 My Role
### 1. 담당 업무
![image](https://user-images.githubusercontent.com/85034286/153706245-23cdeb49-92d6-48b9-8bb6-360f4065692d.png)

#### 1-1. 학생 > 출결관리
* 학생이 본인의 출결을 조회 할 수 있다. 출석 시 Sysdate를 기준으로 지각인지 확인한다. 이후 퇴근시 Sysdate를 확인해서 조퇴인지 확인한다 (이미 지각이 적힌 경우 조퇴를 해도 변화하지 않는다.)

![image](https://user-images.githubusercontent.com/85034286/153701974-09650a46-9cbd-4e22-b383-34aa81c4c37b.png)

#### 1-2. 선생님 > 학생 팀 CRUD
* 선생님은 본인 강의를 수강중인 학생의 팀을 조작할 수 있다.

![image](https://user-images.githubusercontent.com/85034286/153707857-cbacf823-5cb1-4b00-826a-fa67b266ea38.png)

#### 1-3. 학생 > 학생 정보 확인
* 희망 취업 정보 및 수강중인 수업의 정보를 확인할 수 있다.

![image](https://user-images.githubusercontent.com/85034286/153708338-4c1e83d2-fb33-41a8-bbb1-301eae587b5a.png)



<br />
<br />



# 🔎 Detail
### 1. 주요 코드
* 해당 코드는 본인의 교윳을 수강중인 교육생의 팀을 검색하는 코드로써 중요한 부분인 begin ~ exception 까지만 나타내 보았습니다.
    ```sql
    ...
    begin
    select teacher_seq into vcheck from tblteacher where teacher_seq = (select teacher_seq from tblteacher where pid = id and jumin = ppw);
    open vcursor;
        loop
            fetch vcursor into name , seq , team;
            exit when vcursor%notfound;
            dbms_output.put_line('이름:'||name || '  수강 번호: ' || seq || '  팀: ' || team);
        end loop;
    close vcursor;
    exception
    when others then
    dbms_output.put_line('              권한이 없습니다. 로그인 정보를 다시 확인하세요');
    ...
    ```
* 해당 코드는 본인의 강의를 수강중인 학생의 팀을 지정하는 코드로써 중요한 부분인 begin ~ exception 까지만 나타내 보았습니다.
    ```sql
    ...
    begin
    select teacher_seq into seq from tblteacher where teacher_seq = (select teacher_seq from tblteacher where id = pid and jumin = ppw);
    select count(*) into vcheck1 from tblteam where sugang_seq = student1;
    select count(*) into vcheck2 from tblteam where sugang_seq = student2;
    select count(*) into vcheck3 from tblteam where sugang_seq = student3;
    select count(*) into vcheck4 from tblteam where sugang_seq = student4;
    select count(*) into vcheck5 from tblteam where sugang_seq = student5;
    select count(*) into vcheck6 from tblteam where sugang_seq = student6;
    if seq>0 then
        if vcheck1 = 0 then
            insert into tblTeam VALUES ((select team_seq+1 from(select team_seq from tblteam order by team_seq desc) where rownum = 1),t,student1);
            dbms_output.put_line('sugang_seq : '||student1||' 학생 추가 완료');
        else
            dbms_output.put_line('sugang_seq : '||student1||' 학생 은 이미 팀이 구성되어 있습니다.');
        end if;
        if vcheck2 = 0 then
            insert into tblTeam VALUES ((select team_seq+1 from(select team_seq from tblteam order by team_seq desc) where rownum = 1),t,student2);
            dbms_output.put_line('sugang_seq : '||student2||' 학생 추가 완료');
         else
            dbms_output.put_line('sugang_seq : '||student2||' 학생 은 이미 팀이 구성되어 있습니다.');
            end if;
        if vcheck3 = 0 then
            insert into tblTeam VALUES ((select team_seq+1 from(select team_seq from tblteam order by team_seq desc) where rownum = 1),t,student3);
            dbms_output.put_line('sugang_seq : '||student3||' 학생 추가 완료');
        else
            dbms_output.put_line('sugang_seq : '||student3||' 학생 은 이미 팀이 구성되어 있습니다.');
        end if;
        if vcheck4 = 0 then
            insert into tblTeam VALUES ((select team_seq+1 from(select team_seq from tblteam order by team_seq desc) where rownum = 1),t,student4);
            dbms_output.put_line('sugang_seq : '||student4||' 학생 추가 완료');
        else
            dbms_output.put_line('sugang_seq : '||student4||' 학생 은 이미 팀이 구성되어 있습니다.');
        end if;
        if vcheck5 = 0 then
            insert into tblTeam VALUES ((select team_seq+1 from(select team_seq from tblteam order by team_seq desc) where rownum = 1),t,student5);
            dbms_output.put_line('sugang_seq : '||student5||' 학생 추가 완료');
        else
            dbms_output.put_line('sugang_seq : '||student5||' 학생 은 이미 팀이 구성되어 있습니다.');
        end if;
        if vcheck6 = 0 then
            insert into tblTeam VALUES ((select team_seq+1 from(select team_seq from tblteam order by team_seq desc) where rownum = 1),t,student6);
            dbms_output.put_line('sugang_seq : '||student6||' 학생 추가 완료');
          else
            dbms_output.put_line('sugang_seq : '||student6||' 학생 은 이미 팀이 구성되어 있습니다.');end if;
    end if;
    exception
        when others then
        dbms_output.put_line('              권한이 없습니다. 로그인 정보를 다시 확인하세요');
    end;
    ...
    ```
* 해당 코드는 학생이 본인의 출결을 관리하는 코드로써 중요한 부분인 begin ~ exception 까지만 나타내 보았습니다.
    ```sql
    ...
    begin
    select 1 into vcheck  from tblsugang su where su.student_seq = ( select stu.student_seq from tblstudent stu where stu.id = pid and substr(stu.ssn,7) = ppw);
    open vcursor;
        LOOP
            fetch vcursor into studentName ,attendDate  ,absenceType ,vGoToWork ,vOffWork;
            exit when vcursor%notfound;
           dbms_output.put_line(' 이름 : '||studentName||' | 출석 '||attendDate||' | 출결 종류 : '||absenceType||' | 입실 시간 : '||vGoToWork||' | 퇴실 시간 : '||vOffWork);
         end loop;
    close vcursor;
    exception
        when others then
        dbms_output.put_line('              권한이 없습니다. 로그인 정보를 다시 확인하세요');
    end;
    ...
    ```
* 해당 코드는 학생이 출석을 했을때 작동하는 코드로써 중요한 부분인 begin ~ exception 까지만 나타내 보았습니다.
    ```sql
    begin
    select 1 into vcheck  from tblsugang su where su.student_seq = ( select stu.student_seq from tblstudent stu where stu.id = pid and substr(stu.ssn,7) = ppw);
    if vcheck <> 0 then
        insert into tblAttendence(Attendence_Seq, absence_type, Attendence_Date, Sugang_Seq , GoTowork)
        select 
            (select * from (select attendence_seq from tblAttendence order by attendence_seq desc) where rownum =1)+1,
            case
                when to_char(sysdate , 'HH24:MM')> '09:00' then '지각'
                when to_char(sysdate , 'HH24:MM')< '09:00' then '기타'
            end,
            to_char(sysdate , 'yyyy-mm-dd'),
            su.sugang_seq,
            sysdate
        from tblsugang su
            where su.student_seq = ( select stu.student_seq from tblstudent stu where stu.id = pid and substr(stu.ssn,7) = ppw) ;
        dbms_output.put_line('출근완료');
    end if;
    exception
        when others then
        dbms_output.put_line('              권한이 없습니다. 로그인 정보를 다시 확인하세요');
    end;
    ```    
    
* 해당 코드는 학생이 퇴근 했을때 작동하는 코드로써 중요한 부분인 begin ~ exception 까지만 나타내 보았습니다.
    ```sql
    begin
    select 1 into vcheck  from tblsugang su where su.student_seq = ( select stu.student_seq from tblstudent stu where stu.id = pid and substr(stu.ssn,7) = ppw);
    if vcheck <> 0 then
        update tblAttendence set offwork = sysdate, 
                         absence_type = ( select 
                                                case
                                                    when  ABSENCE_TYPE = '지각' then '지각'
                                                    when  to_char(sysdate , 'HH24:MM')< '18:00' then '조퇴'
                                                    when  to_char(sysdate , 'HH24:MM')> '18:00' then '정상'
                                                end
                                            from tblAttendence 
                                                where ATTENDENCE_SEQ = (select 
                                                                            * 
                                                                        from (select 
                                                                                ATTENDENCE_SEQ 
                                                                              from tblsugang s inner join tblAttendence a on s.sugang_seq = a.sugang_seq 
                                                                              where student_seq  = ( select 
                                                                                                        stu.student_seq 
                                                                                                     from tblstudent stu 
                                                                                                        where stu.id = pid and substr(stu.ssn,7) = ppw) 
                                                                              order by attendence_seq desc) 
                                                                        where rownum =1)) -- 수정해야될 행의 출근시간 참조
                                                where ATTENDENCE_SEQ = (select 
                                                                            * 
                                                                        from (select 
                                                                                ATTENDENCE_SEQ 
                                                                              from tblsugang s inner join tblAttendence a on s.sugang_seq = a.sugang_seq 
                                                                              where student_seq  = ( select 
                                                                                                        stu.student_seq 
                                                                                                     from tblstudent stu 
                                                                                                        where stu.id = pid and substr(stu.ssn,7) = ppw) 
                                                                              order by attendence_seq desc) 
                                                                       where rownum =1); -- 로그인 한 학생이 마지막에 남김 정보
       dbms_output.put_line('퇴근완료');
     end if;
    exception
    when others then
    dbms_output.put_line('              권한이 없습니다. 로그인 정보를 다시 확인하세요');
    end;
    ```    
<br />
<br />
<br />

# 💡 Review
### 1. 후기
* DB 작업시 기초 ERD 설계가 정말 중요하고 , 해당 과정이 조금이라도 잘못 짜여졌을시 그 후에 파장이 정말 크다는것을 몸소 느끼게 된 계기가 되었습니다.
* 각각의 오라클 환경에서 구현하고 쿼리는 모으를 방식으로 협업을 진행했는데 ORCLE cloud 를 사용했으면 더 효과적으로 협업을 진행할 수 있었을텐데.. 라는 아쉬음이 있습니다.

<br />
<br />
